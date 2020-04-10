可以通过终端命令ls -l查看，这些文件通常都会有@作为标记
1.可以针对单个文件做清除操作（filename就是要文件名，例如：index.html）
```
xattr -c filename
```
2.也可以针对整个目录做清除操作（directory就是目录名，例如：content）
```
xattr -rc directory
```
