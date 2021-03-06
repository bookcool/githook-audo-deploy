
## git-auto-deploy

### Abstract

Auto deploy when pushed code to git;  代码推送自动部署;

using git-hook and spring-boot; 使用git-hook 与spring-boot实现;

发布机器上部署本服务，通过github(gitlab) web hook push event 触发回调本服务api，执行用户配置的发布脚本。

### Feature

多用户、多项目的权限控制，

重复事件过滤机制;

邮件提醒功能;

### Getting started ：
1. write deploy shell 自己写好发布脚本(拉取最新代码、打包、重启等, 放在项目目录下)

    Eg: ~/project/deploy.sh

2. start 启动本服务 (DNS: ip:port -> domain) 
    
    Eg: java -jar githook-deploy-1.0-SNAPSHOT.jar --server.port=8888 > auto-deploy.log 2>&1 &

3. set git hook git服务器上或git web hook 配置hook调用本服务接口 
    
    配置页面
    git: Setting -> Webhooks
    gitlab: Setting -> Integrations -> Webhooks
    
    配置事件触发调用的url:
    POST /deploy/{project}
    (project为项目部署目录名)

4. set deploy shell path 配置发布脚本的路径 

    deploy.shell.path.template (配置文件: application.properties)
    
    或调用此接口每个项目单独配置动态修改 
    POST /deploy/{project}/deploy/shell?path=xxx

5. set deploy get branch 设置需要自动发布的分支 

    auto.deploy.branch (配置文件: application.properties)
    
    或调用接口动态修改 POST /deploy/git/branch?branch=xxx (default默认 deploy-test)

6. (optional 可选) auto deploy on-off 每个项目的自动发布开关控制 
    
    POST /deploy/{project}/auto/deploy?enable=false (默认开启)

### TODO
extension points : pre/post deploy 重启前、后的扩展点，Eg: send mail to submitter after deploy 重启完成后发送邮件通知提交者。
