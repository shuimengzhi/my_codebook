mac安装polipo将socks5与http互相转换
```
brew install polipo
```
在.bash或者.zshrc写入
```
alias poff="unset http_proxy; unset https_proxy"
alias pon="export http_proxy=http://127.0.0.1:8123;export https_proxy=http://127.0.0.1:8123"
alias pon_run="polipo proxyAddress="127.0.0.1" socksParentProxy=127.0.0.1:1080"
```
端口8123是socks5转成http后的端口，平时只要输入`pon_run`,然后再开一个终端输入`pon`,就好了。输入`poff`关闭代理

socks5  1080端口是酸酸乳客户端本地开启的本地代理

如果要docker也用代理，则在http_proxy输入127.0.0.1:8123就好了，这样容器里面`curl www.google.com`是成功的

第二种开启方法，直接终端输入下面命令就好
```
polipo socksParentProxy=127.0.0.1:1080 proxyAddress="127.0.0.1"
```
