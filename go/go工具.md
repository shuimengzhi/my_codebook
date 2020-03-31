#数据表生成gorm model
```
go get github.com/smallnest/gen

gen --connstr "root:root@tcp(127.0.0.1:3308)/shui_oauth?charset=utf8mb4&parseTime=True&loc=Local" --database shui_oauth --json --gorm --guregu --rest

```