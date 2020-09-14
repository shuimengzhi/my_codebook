yum -y install bind
vim /etc/named.conf
```
options {
	listen-on port 53 { 172.16.183.103; };//本地机子的ip，用于监听
	listen-on-v6 port 53 { ::1; };
	directory 	"/var/named";
	dump-file 	"/var/named/data/cache_dump.db";
	statistics-file "/var/named/data/named_stats.txt";
	memstatistics-file "/var/named/data/named_mem_stats.txt";
	secroots-file	"/var/named/data/named.secroots";
	recursing-file	"/var/named/data/named.recursing";
	allow-query     { any; };//允许查询任何ip
    forwarders      {172.16.183.2; };//网关ip，向上查询使用,也可以是公共的dns ip

	/*
	 - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
	 - If you are building a RECURSIVE (caching) DNS server, you need to enable
	   recursion.
	 - If your recursive DNS server has a public IP address, you MUST enable access
	   control to limit queries to your legitimate users. Failing to do so will
	   cause your server to become part of large scale DNS amplification
	   attacks. Implementing BCP38 within your network would greatly
	   reduce such attack surface
	*/
	recursion yes;

	dnssec-enable no; //正常是yes，no是为了节省空间
	dnssec-validation no; //正常是yes，no是为了节省空间

	managed-keys-directory "/var/named/dynamic";

	pid-file "/run/named/named.pid";
	session-keyfile "/run/named/session.key";

	/* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
	include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
	type hint;
	file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";

```

vim /etc/named.rfc1912.zones
```
zone "localhost.localdomain" IN {
	type master;
	file "named.localhost";
	allow-update { none; };
};

zone "localhost" IN {
	type master;
	file "named.localhost";
	allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
	type master;
	file "named.loopback";
	allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
	type master;
	file "named.loopback";
	allow-update { none; };
};

zone "0.in-addr.arpa" IN {
	type master;
	file "named.empty";
	allow-update { none; };
};

zone "shui.com" IN {
	type master; // 代表主dns
	file "shui.com.zone"; // 自己手动创建 /var/named/shui.com.zone文件
	allow-update { 172.16.183.103; };
};
```
cp -rp named.localhost shui.com.zone
为了权限相同

vim /var/named/shui.com.zone
```
$TTL 1D
@	IN SOA	dns.shui.com. rname.invalid. (
					0	; serial
					1D	; refresh
					1H	; retry
					1W	; expire
					3H )	; minimum
	NS	dns.shui.com.
dns	A	172.16.183.103 //本地ip
node2	A       172.16.183.102 //需要解析的ip,node2是node2.shui.com
node1   A  	172.16.183.101
master  A       172.16.183.100
```
systemctl restart named
netstat -luntp|grep 53
dig -t A node1.shui.com @172.16.183.103 +short
测试是否成功
vim /etc/sysconfig/network-scripts/ifcfg-ens33
重新配置网络
ifup ens33
重启网卡

防火墙记得开启53端口的udp并且重启
firewall-cmd --add-port=53/udp --permanent
firewall-cmd --reload