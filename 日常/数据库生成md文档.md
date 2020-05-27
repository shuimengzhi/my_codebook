# Install
```
# build install
git clone https://github.com/alicfeng/mysql_markdown.git
cd mysql_markdown
go get "github.com/go-sql-driver/mysql"
go build -o /usr/local/bin/mysql_markdown mysql_markdown.go
chmod +x /usr/local/bin/mysql_markdown
```

# Use
```
mysql_markdown -h
flag needs an argument: -h
Usage: mysql_markdown [options...]
--help  This help text
-h      host.     default 127.0.0.1
-u      username. default root
-p      password. default root
-d      database. default mysql
-P      port.     default 3306
-c      charset.  default utf8
-o      output.   default current location
```
mysql_markdown -u root -p root -P 3308 -d douyin