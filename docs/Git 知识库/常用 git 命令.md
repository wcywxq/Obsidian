---
title: 常用 git 命令
url: https://www.yuque.com/wcywxq/uv8hkw/bis7pd
---

- `master`：默认开发分支
- `origin`：默认远程版本库
- `Head`：默认开发分支
- `Head^`：`Head` 的父提交 <a name="WFJfy"></a>

## 修改版本号

- `npm version major` 主版本号 +1，其余版本号归 0
- `npm version minor` 次版本号 +1，修订号归 0
- `npm version patch` 修订号 +1
- `npm version 版本号` 设置版本号为指定的版本号
- `npm version prerelease` 先行版本号 +1
- `npm version prerelease --preid=alpha` 假设现在的 `version` 是 `1.3.5`，执行完该命令之后就会成为 `1.3.6alpha.0`

<a name="iFi8y"></a>

## 查看包版本

- `npm view 包名称 versions`，查看 `npm` 服务器上所有的包版本信息
- `npm view 包名称 version`，查看最新的包版本
- `npm info 包名称`，可以查处更多关于包的信息，包括包的版本信息

<a name="CFg1w"></a>

## 创建版本库

- 克隆远程版本库：`git clone <url>`
- 初始化本地版本库：`git init`

<a name="LFfpb"></a>

### 修改和提交

- 查看状态：`git status`
- 查看变更内容：`git diff`
- 跟踪所有改动过的文件：`git add`
- 跟踪指定的文件：`git add <file>`
- 文件改名：`git mv <old> <new>`
- 删除文件：`git rm <file>`
- 停止跟踪文件但不删除：`git rm --cached <file>`
- 提交所有更新过的文件：`git commit -m "commit message"`
- 修改最后一次提交：`git commit --amend` <a name="FFBVF"></a>

### 查看提交历史

- 查看提交历史：`git log`
- 查看指定文件的提交历史：`git log -p <file>`
- 以列表方式查看指定文件的提交历史：`git blame <file>`

<a name="I3sew"></a>

### 撤销

- 撤销工作目录中所有未提交文件的修改内容：`git reset --hard HEAD`
- 撤销指定的未提交文件的修改内容：`git checkout HEAD <file>`
- 撤销指定的提交：`git revert <commit>`

<a name="N0MhU"></a>

### 分支与标签

- 显示所有本地分支：`git branch`
- 切换到指定分支或标签：`git checkout <branch/tag>`
- 创建新分支：`git branch <new-branch>`
- 删除本地分支：`git branch -d <branch>`
- 列出所有本地标签：`git tag`
- 基于最新提交创建标签：`git tag <tagname>`
- 删除标签：`git tag -d <tagname>`

<a name="J7l2D"></a>

### 合并与衍合

- 合并指定分支到当前分支：`git merge <branch>`
- 衍合指定分支到当前分支：`git rebase <branch>`

<a name="VM8d5"></a>

### 远程操作

- 查看远程版本库信息：`git remote -v`
- 查看指定远程版本库信息：`git remote show <remote>`
- 添加远程版本库：`git remote add <remote> <url>`
- 从远程库获取代码：`git fetch <remote>`
- 下载代码及快速合并：`git pull <remote> <branch>`
- 上传代码及快速合并：`git push <remote> <branch>`
- 删除远程分支或标签：`git push <remote> :<branch/tag-name>`
- 上传所有标签：`git push --tags`
