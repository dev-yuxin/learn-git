# 环境相关

下载安装 git : https://git-scm.com

```bash
# 全局初始化配置只需一次，用户名和邮箱有空格需用引号包裹
git config --global user.name <用户名>
git config --global user.email <邮箱>
```

注册 github : https://github.com

配置 ssh 密钥：

```bash
# 一直按回车在用户家目录生成 .ssh 里面有公钥和私钥
ssh-keygen -t ed25519 
```

把公钥复制到 github 上。

# 常用命令

## 仓库相关

```bash
# 克隆远程仓库
git clone <远程仓库地址>

# 初始化一个本地仓库
git init

# 设置远程仓库，一般取个别名 origin
git remote add origin <远程仓库地址>
```

## 提交相关

```bash
# 把文件从工作区添加到暂存区
git add <文件>
# 把所有文件都添加到暂存区
git add .

# 把暂存区中所有文件（只动暂存区，不动工作区）提交到本地仓库
git commit -m <提交信息>

# 如果本地分支已和远程关联，可简写 git push
# 未关联则需执行一次 git push -u <远程仓库> <本地分支>[:<远程分支>]
git push
git push -u <远程仓库> <本地分支>[:<远程分支>]
```

## 拉取相关

```bash
# git fetch 拉取后并更新本地 origin/* 追踪的分支，不会改工作区
# -p/-prune 清理本地已失效的 origin/* 追踪的分支
git fetch
git fetch -p

# git pull = git fetch + git merge 会改工作区文件（可能冲突）
# 拉取当前分支关联的远程分支，进行合并
git pull
# 拉取远程 master 分支并合并到当前分支
git pull origin master
# 合并采用 reabase 模式
git pull --rebase
```

## 分支相关

```bash
# 查看所有分支
git branch

# 创建分支
git branch <分支名>

# 切换分支
git checkout <分支名>

# merge 合并
# 切到 b 分支，再把 a 合并到 b
git checkout b
git merge a

# rebase 变基合并
# 切到 a 分支，再把 a 分支变基合并到 b，把 a 分支独有的提交内容移动到 b 上
git checkout a
git rebase b

# 当合并到 master/main/release 形成一个比较稳定的版本时，打一个 tag 标签
git tag v1.0.0

# 删除已合并分支
git branch -d <分支名>
# 强制删除分支
git branch -D <分支名>
# 把远程的分支也删除
git push origin --delete <分支名>
```

## 回退相关

```bash
# 查看 commit 提交记录（能查看到 commit 的 hash 值）
# --oneline 简化版
git log
git log --oneline
# 查看本机的提交记录（被回退的 commit 也能看见）
git reflog

# 回退到某次提交的版本
# --mixed（或不写）工作区保留，暂存区不保留
# --soft 工作区和暂存区都保留
# --hard 工作区和暂存区都不保留（谨慎）
git reset <commit_hash>
git reset <commit_hash> --soft
git reset <commit_hash> --hard

# commit1 -> commit2 -> commit3 -> ...
# 如果想撤回 commit 的修改但想保留 commit2, commit3 等后面的修改，使用 revert 新建一个反向提交
git revert <commit1_hash>
```

# .gitignore

一些文件是不应该放入 git 的。比如：

- 系统或软件自己生成的
- 编译产生的中间文件

- 依赖相关
- 存密钥等敏感信息文件
- 日志文件

需要在项目根目录下添加 .gitignore 文件。

需要注意 .gitgnore 文件只对工作区文件生效，已经被添加到暂存区或已提交的文件需额外操作：

```bash
git rm --cache <文件>
```

常用的项目 .gitgnore 文件：https://github.com/github/gitignore

忽略规则：https://git-scm.cn/docs/gitignore

# gitflow

分支规范：

- master/main : 生产分支，有权限管控，不能直接提交，必须提 Merge 。
- release : 待上线分支
- pre/beta : 预发环境分支
- dev/test : 测试环境分支
- feature_xxx : 新增功能分支
- hotfix_xxx : 修复分支

commit 信息规范：

- "fix: 修复了什么什么内容"
- "feat: 新增了什么什么内容"
