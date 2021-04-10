# php开始
## 开始阶段
- 模块初始化阶段，该过程只执行一次
- 模块激活阶段，也就是请求阶段
- 请求到达后，初始化php脚本运行环境

# 多进程sapi
php通常会编译为apache的模块
# SAPI(Server Application Programming Interface)
SAPI用于php与服务器的通信，例如nginx,apache
# fast-cgi
fast-cgi能将cgi解释器常驻内存
工作流：
1.web server启动加载fast-cgi进程管理器
2.fast-cgi初始化，启动多个cgi解释器进程
3.请求来的时候，web server将cgi环境变量和标准输入发送到fast-cgi子进程php-cgi
4.fast-cgi子进程处理完返回给web server，子进程继续等待请求
