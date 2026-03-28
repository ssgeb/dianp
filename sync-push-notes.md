# GitHub 提交流程

## 提交目标

- 目标仓库：`https://github.com/ssgeb/dianp.git`
- 当前仓库结构：
  - 前端项目：仓库根目录
  - 后端项目：`hm-dianping/`

## 操作命令

1. 查看远端仓库是否可访问
   - `git ls-remote https://github.com/ssgeb/dianp.git`
2. 添加 GitHub 远端
   - `git remote add github https://github.com/ssgeb/dianp.git`
3. 先拉取远端信息，确认远端分支
   - `git fetch github`
4. 暂存前端和后端项目文件
   - `git add hm-dianping sync-push-notes.md`
   - 另外把前端静态资源图片一起纳入提交
5. 提交本地改动
   - `git commit -m "同步前后端项目并记录部署说明"`
6. 推送到 GitHub
   - `git push github master:main --force-with-lease`

## 遇到的问题

### 1. Git 走本地代理时，TLS 握手被重置

- 现象：推送时报 `Recv failure: Connection was reset`
- 原因：Git 通过本地代理 `127.0.0.1:7890` 连接 GitHub，TLS 握手阶段被重置
- 解决方法：在推送命令前临时清空代理变量，并显式关闭 Git 的代理配置

### 2. GitHub 远端历史和本地历史不是同一条线

- 现象：`github/main` 和本地 `master` 无法直接快进
- 解决方法：使用 `--force-with-lease` 进行受保护的强制更新

## 最终推送命令

```powershell
$env:HTTP_PROXY=''
$env:HTTPS_PROXY=''
$env:ALL_PROXY=''
$env:http_proxy=''
$env:https_proxy=''
$env:all_proxy=''
git -c http.proxy= -c https.proxy= push github master:main --force-with-lease
```

## 结果

- GitHub `main` 已更新到本地提交 `0054e50`
- 仓库中同时包含前端静态项目和后端 `hm-dianping/` 项目
