---
title: verdaccio
url: https://www.yuque.com/wcywxq/mxunh7/yms8mi
---

[verdaccio](https://verdaccio.org/zh-CN/docs/what-is-verdaccio) 是一个 `Node.js` 创建的轻量的私有 `npm proxy registry` <a name="wXONu"></a>

### docker 部署 verdaccio

```shell
# 拉取verdaccio docker镜像
$ docker pull verdaccio/verdaccio:nightly-master

# 查看docker镜像
$ docker images
REPOSITORY                                TAG              IMAGE ID       CREATED        SIZE
verdaccio/verdaccio                       nightly-master   32713721fda5   16 hours ago   580MB

# 拷贝下面配置文件到本地~/docker/verdaccio目录
$ cp config.yaml ~/docker/verdaccio

# 启动docker容器
# -d: 在后台开启docker进程
# --name: 给容器指定一个名称
# --p: 将本机的4873端口映射到docker的4873端口
# --restart=always: 自动重启容器
# -v: 将本地~/docker/verdaccio目录挂载到docker的/verdaccio/conf目录
$ docker run --restart=always -d -v ~/docker/verdaccio:/verdaccio/conf --name verdaccio -p 4873:4873 verdaccio/verdaccio

# 查看docker容器
$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                       NAMES
6aac1ea8707a   verdaccio/verdaccio   "uid_entrypoint /bin…"   2 minutes ago   Up 2 minutes   0.0.0.0:4873->4873/tcp, :::4873->4873/tcp   verdaccio
```

<a name="gjQ5e"></a>

### pm2 部署 verdaccio

```shell
# 全局安装 verdaccio和pm2
$ npm install -g verdaccio pm2
# 通过pm2启动verdaccio
$ pm2 start verdaccio
[PM2] Starting /usr/local/bin/verdaccio in fork_mode (1 instance)
[PM2] Done.
┌─────┬──────────────┬─────────────┬─────────┬─────────┬──────────┬────────┬──────┬───────────┬──────────┬──────────┬──────────┬──────────┐
│ id  │ name         │ namespace   │ version │ mode    │ pid      │ uptime │ ↺    │ status    │ cpu      │ mem      │ user     │ watching │
├─────┼──────────────┼─────────────┼─────────┼─────────┼──────────┼────────┼──────┼───────────┼──────────┼──────────┼──────────┼──────────┤
│ 0   │ verdaccio    │ default     │ N/A     │ fork    │ 20889    │ 0s     │ 0    │ online    │ 0%       │ 10.2mb   │ cm       │ disabled │
└─────┴──────────────┴─────────────┴─────────┴─────────┴──────────┴────────┴──────┴───────────┴──────────┴──────────┴──────────┴──────────┘
```

<a name="xw0BR"></a>

### 扩展

如果想要跟gitlab或者github等代码管理工具来关联权限，可以参考下社群的开源插件。如果需要根据自身业务进行权限管理的话，可以尝试下自己实现权限管理插件，具体参考[权限控制插件](https://www.verdaccio.org/docs/en/plugin-auth)

- verdaccio-bitbucket：verdaccio Bitbucket 认证插件。
- verdaccio-bitbucket-server：verdaccio 的 Bitbucket 服务器身份验证插件。
- verdaccio-ldap：verdaccio 的 LDAP 身份验证插件。
- verdaccio-active-directory：verdaccio 的 Active Directory 身份验证插件
- verdaccio-gitlab：使用 GitLab 个人访问令牌进行身份验证
- verdaccio-gitlab-ci：启用 GitLab CI 来针对 verdaccio 进行身份验证。
- verdaccio-htpasswd：基于 htpasswd 文件插件（内置）的 verdaccio 身份验证
- verdaccio-github-oauth：verdaccio 的 Github oauth 认证插件。
- verdaccio-github-oauth-ui：verdaccio 登录按钮的 GitHub OAuth 插件。
- verdaccio-groupnames：用于使用 $group 语法处理动态组关联的插件。与 ldap 插件配合使用效果最佳。
