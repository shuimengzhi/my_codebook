```
kind: pipeline
type: ssh
name: default

server:
  host: 服务器地址ip或者域名
  user: 
    from_secret: username（这个值在drone服务端配置）
  password:
    from_secret: password（这个值在drone服务端配置）
platform:
    os: linux
    arch: amd64

steps:
- name: greeting
  commands:
  # - /usr/bin/expect /data/test2.sh 
  # - exit
  # - /usr/bin/bash /data/test3.sh
  - echo "1">>/data/log/4.txt
  when:
    # event: 
    #   # - push
    #   - tag
    ref:
      include:
        - refs/tags/services/*
# trigger:
#   ref:
#     - refs/tags/product-v*
#   branch:
#     include:
#       - master
```
ref不与branch同时使用，否则无法触发自动部署
触发部署方法
```
git tag services/v1
git push origin master tag services/v1
```
项目文件.git下面的refs/tags存放着各种打过的标签
