# 同步提交记录

## 目标

- 将当前前端静态项目同步提交到 `https://github.com/ssgeb/dianp.git`
- 让本地前端可以正常通过 `nginx` 访问
- 记录启动和排错过程，方便后续复现

## 实际执行步骤

1. 检查仓库状态，确认当前仓库根目录在 `D:\dataset\02-实战篇\资料\nginx-1.18.0\html\hmdp`
2. 检查远端仓库，确认 GitHub 地址可访问：
   - `git ls-remote https://github.com/ssgeb/dianp.git`
3. 启动前端静态服务时，使用英文别名路径规避中文目录编码问题：
   - 先创建 `D:\hm-nginx` 指向 `D:\dataset\02-实战篇\资料\nginx-1.18.0` 的联接
   - 再执行：
     - `D:\hm-nginx\nginx.exe -p D:\hm-nginx -c conf\nginx.conf`
4. 检查前端首页是否正常响应：
   - `Invoke-WebRequest http://127.0.0.1:8080/`
5. 整理本仓库新增的图片资源：
   - `imgs/blogs/...`
6. 将本次说明文件一起提交到仓库
7. 将本地分支推送到 GitHub 的 `main` 分支

## 遇到的问题

### 1. `nginx` 在中文路径下启动失败

- 现象：`nginx` 报 `No mapping for the Unicode character exists in the target multi-byte code page`
- 原因：Windows 下 `nginx` 对中文路径兼容性不好
- 解决方法：使用英文别名路径 `D:\hm-nginx` 启动，并显式传入 `-p` 和 `-c`

### 2. 前端图片缺失

- 现象：前端页面里部分图片不显示
- 原因：
  - 静态图片依赖 `nginx` 提供
  - 动态内容图片依赖后端接口返回
  - 后端图片上传目录原来写死成了旧机器路径
- 解决方法：
  - 把后端图片目录改为当前环境可用路径
  - 修正上传与删除图片时的相对路径处理

### 3. 远端分支差异

- 现象：GitHub 远端默认分支是 `main`，本地仓库分支是 `master`
- 解决方法：推送时使用 `master:main`

## 结果

- 前端首页已可通过 `http://127.0.0.1:8080/` 访问
- 当前仓库内容已整理完毕，准备推送到 GitHub

## 备注

- 当前 Git 仓库是前端静态站点仓库
- 后端代码修复在另一个目录中完成，但不属于这个 Git 仓库的跟踪范围
