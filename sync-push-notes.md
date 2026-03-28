# GitHub 提交流程记录

## 提交目标

- 仓库地址：`https://github.com/ssgeb/dianp.git`
- 目录结构：
  - `hm-dianping`：后端项目文件夹
  - `nginx-1.18.0`：前端项目文件夹

## 操作命令

1. 查看远端仓库

```powershell
git ls-remote https://github.com/ssgeb/dianp.git
```

2. 添加 GitHub 远端

```powershell
git remote add github https://github.com/ssgeb/dianp.git
```

3. 拉取远端信息

```powershell
git fetch github
```

4. 整理目录结构，把前端项目放到 `nginx-1.18.0`，把后端项目放到 `hm-dianping`

```powershell
Move-Item css, imgs, js, *.html, favicon.ico -Destination .\nginx-1.18.0\html\hmdp\
```

5. 暂存改动

```powershell
git add .
```

6. 提交本地变更

```powershell
git commit -m "合并前后端项目到单仓库"
```

7. 推送到 GitHub

```powershell
git -c http.version=HTTP/1.1 push github master:main --force-with-lease
```

## 遇到的问题

### 1. Git 走本地代理时连接不稳定

- 现象：推送时出现 `Recv failure: Connection was reset`
- 原因：Git 默认走本地代理 `127.0.0.1:7890`，连接 GitHub 时不稳定
- 解决方法：保留代理出口，但强制 Git 使用 `HTTP/1.1`，避免 HTTP/2 引发的中途断开

### 2. 本地仓库结构和目标仓库结构不一致

- 现象：前端项目原本直接放在仓库根目录
- 解决方法：把前端静态文件整体移动到 `nginx-1.18.0/html/hmdp`，让仓库根目录只保留两个项目文件夹

### 3. GitHub 远端历史和本地历史不同步

- 现象：本地提交和 GitHub `main` 分支不是同一条历史线
- 解决方法：使用 `--force-with-lease` 做受控强推，安全地覆盖远端分支

## 结果

- GitHub 仓库已同步为两个顶层文件夹：
  - `hm-dianping`
  - `nginx-1.18.0`
- 本次提交说明仅保留 GitHub 提交流程、命令和排错方法
