---
title: verdaccio
url: https://www.yuque.com/wcywxq/xxble8/tbmwax
---

<a name="9a109a36"></a>

# This is the default config file. It allows all users to do anything,

<a name="5955c16e"></a>

# so don't use it on production systems.

<a name="d41d8cd9"></a>

#

<a name="218b8f6e"></a>

# Look here for more config file examples: 这个 examples 404了。

<a name="10c551f4"></a>

# <https://github.com/verdaccio/verdaccio/tree/master/conf>

<a name="d41d8cd9-1"></a>

#

<a name="981e20ca"></a>

# 仓库的包默认存储的位置，默认是不存在的，当发布私有包之后会在 /root/.config/verdaccio 中存在

<a name="7793648d"></a>

# path to a directory with all packages

storage: ./storage

<a name="d2210cdd"></a>

# path to a directory with plugins to include

plugins: ./plugins

web:

<a name="7703d86a"></a>

# 网页 title

title: Verdaccio

<a name="6662fb00"></a>

# comment out to disable gravatar support

<a name="66bf0561"></a>

# gravatar: false

<a name="6ed3aa95"></a>

# 包的排序 asc 降序 desc 升序

<a name="2e95abc1"></a>

# by default packages are ordercer ascendant (asc|desc)

<a name="6b0734d4"></a>

# sort\_packages: asc

<a name="62b17d5f"></a>

# 是否开启暗黑模式

<a name="bad7120e"></a>

# convert your UI to the dark side

<a name="e79de355"></a>

# darkMode: true

<a name="4028f886"></a>

# 网页 logo

<a name="131e53da"></a>

# logo: <http://somedomain/somelogo.png>

<a name="f2a2e490"></a>

# 网页 favicon

<a name="67f72d3e"></a>

# favicon: <http://somedomain/favicon.ico> | /path/favicon.ico

<a name="eb072733"></a>

# 网页最大连接数

<a name="03958db8"></a>

# rateLimit:

<a name="3b738eb5"></a>

# windowMs: 1000

<a name="82725cb6"></a>

# max: 10000

<a name="73de9879"></a>

# translate your registry, api i18n not available yet

<a name="6042cf32"></a>

# i18n:

<a name="b3a4366c"></a>

# 国际化相关，同样404了。

<a name="cb45fa41"></a>

# list of the available translations <https://github.com/verdaccio/ui/tree/master/i18n/translations>

<a name="47feedd6"></a>

# 支持 zh\_CN

<a name="9aab418d"></a>

# web: en-US

auth:

<a name="c1883f1f"></a>

# 注册用户的信息存放位置

htpasswd:
file: ./htpasswd
\# Maximum amount of users allowed to register, defaults to "+inf".
\# You can set this to -1 to disable registration.
\# 默认可注册的用户是无限的，如果修改为 -1 则是不允许注册；限定用户方法如下
\# 1，先注册 n 个用户，然后修改配置文件为 -1，后续不允许注册
\# 2，开个 n 个用户注册接口
\# max\_users: 1000

<a name="d9facb0c"></a>

# a list of other known repositories we can talk to

<a name="1b8beadd"></a>

# 如果私有库没有的话，去查找以下库。

uplinks:
npmjs:
url: <https://registry.npmjs.org/>

packages:

<a name="51abf42e"></a>

# @*/ 是包的作用域 例如 @vue/cli 那么 [@vue ](/vue) 则是这个包的作用域，前缀一般都是某个组织或者说某个公司，证明这是个内部包

'@*/*':
\# scoped packages
access: $all
publish: $authenticated
unpublish: $authenticated
\# 如果私有库没有的话，去 npm 查找查找库。
proxy: npmjs

<a name="314dad8d"></a>

# 普通的 npm 包

'**':
\# 允许所有用户发布和访问包
\# allow all users (including non-authenticated users) to read and
\# publish all packages
\#
\# 也可以指定指定 @xxx/xxx 类似的作用域
\# you can specify usernames/groupnames (depending on your auth plugin)
\# $all 所有人 $anonymous 只有匿名者可以操作（基本没用） ![](../assets/tbmwax/latex)all", "![](../assets/tbmwax/latex)authenticated"
access: $all

    # allow all known users to publish/publish packages
    # (anyone can register by default, remember?)
    publish: $authenticated
    unpublish: $authenticated

     # if package is not available locally, proxy requests to 'npmjs' registry
    proxy: npmjs

<a name="857fe1ea"></a>

# You can specify HTTP/1.1 server keep alive timeout in seconds for incoming connections.

<a name="eee4413f"></a>

# A value of 0 makes the http server behave similarly to Node.js versions prior to 8.0.0, which did not have a keep-alive timeout.

<a name="a77bb961"></a>

# WORKAROUND: Through given configuration you can workaround following issue <https://github.com/verdaccio/verdaccio/issues/301>. Set to 0 in case 60 is not enough.

server:
keepAliveTimeout: 60

middlewares:
audit:
enabled: true

<a name="d872f760"></a>

# log settings

logs: { type: stdout, format: pretty, level: http }
\#experiments:

<a name="7ec8c7a2"></a>

# # support for npm token command

<a name="09c1a169"></a>

# token: false

<a name="c6d3a93c"></a>

# # disable writing body size to logs, read more on ticket 1912

<a name="05debf59"></a>

# bytesin\_off: false

<a name="85994f7e"></a>

# # enable tarball URL redirect for hosting tarball with a different server, the tarball\_url\_redirect can be a template string

<a name="bf424f66"></a>

# tarball\_url\_redirect: '<https://mycdn.com/verdaccio/>![](../assets/tbmwax/latex){filename}'

<a name="d130e211"></a>

# # the tarball\_url\_redirect can be a function, takes packageName and filename and returns the url, when working with a js configuration file

<a name="219df791"></a>

# tarball\_url\_redirect(packageName, filename) {

<a name="d454fb8f"></a>

# const signedUrl = // generate a signed url

<a name="b726b605"></a>

# return signedUrl;

<a name="cbb184dd"></a>

# }

<a name="e76137da"></a>

# This affect the web and api (not developed yet)

\#i18n:
\#web: en-US

listen: 0.0.0.0:4873
