# git基础

---

## 1. git安装

未完待续

## 2. git配置

通过git配置，可对不同git仓库文件进行区别管理；
命令示例：

```bash
git config --list
```

### 2.1 常用配置项

- user.name
- user.email

### 2.2 global配置

命令样式: `git config --global [options]`

命令示例:

```bash
# 列出现有global配置信息
git config --global -l

git config --global user.name "qiao"
git config --global user.email "qiao@email.com"
```

### 2.3 local配置

命令样式: `git config --local [options]`

命令示例:

```bash
# 列出现有local配置信息
git config --local -l

git config --local user.name "qiao"
git config --local user.email "qiao@email.com"
```

### 2.4 有效配置优先级

git配置生效的优先级，如下逐级降低:

`local配置` --> `global配置` --> `system配置`

## 3. 创建git仓库

`创建git仓库` = `创建仓库目录` + `git初始化`

### 3.1 本地git仓库创建

1. 创建本地仓库目录
2. 进入参考目录，执行命令:`git init`

### 3.2 远程git仓库创建

通过github/gitlab等web页面的指引创建远程git仓库；

### 3.3 关联远程git仓库

1. 将 远程仓库 clone 到 本地

    > 1. 进入仓库存放目录;
    > 2. 执行clone命令: `git clone 远程仓库的URL`

2. 将 本地仓库 关联到 远程仓库

   > 1. 进入/初始化本地仓库
   > 2. 将本地仓库关联远程仓库: `git remote add origin [远程仓库的git地址].git`

## 4. 文件版本管理

### 4.1 基础概念

- `工作区`: 由未进行版本追踪(untracked)的文件(常驻工作区) 和 含有最新修改但未暂存(git add)的版本追踪(tracked)文件组成；

- `暂存区`: 由已暂存(git add)的版本追踪(trancked)文件组成，git会忽略空目录；

- `历史版本`: 通过git commit 暂存区内容，获得一个历史版本；

- `里程碑(tag)`: 某个历史版本的标签；

- `HEAD指针`: 指向仓库状态正处于某一分支的某一个历史演进版本；

- `分支(branch)`: 代表仓库内容的一个演进历史，演进历史由各个时间历史版本组成；

- `仓库`: 包含 所有文件内容 和 所有分支的文件版本演进历史；

### 4.2 文件版本管理逻辑

1. 单分支演进逻辑

    工作区(未版本追踪的源码文件 `-->` 有修改的版本追踪文件) `==(git add)==>` 暂存区 `==(git commit)==>` 历史版本 `==(git push)==>` 推送至仓库

2. HEAD指针

    `HEAD指针`: 指向某一分支(branch)的某一历史版本(commit)；

3. 分支(branch)演进逻辑

    > - 各分支符合单分支演进逻辑；
    >  
    > - `新分支创建(git checkout)`: 根据当前HEAD指向的文件版本，创建一个新的分支；
    >  
    > - `已有分支切换(HEAD指向切换)`: HEAD: A分支的某一历史版本(commit-id-a) `==(git checkout)==>` HEAD: B分支的某一历史版本(commit-id-b)

4. 里程碑(tag)逻辑

    为某一次重要提交(commit-id), 加上里程碑(tag)；

### 4.3 基本操作

1. 查看仓库的当前状态: `git status`
2. 查看仓库的版本历史: `git log`、`git log -a -v --graph`
3. 拉取远程仓库内容: `git pull`
4. 对文件进行版本管理(将文件添加到暂存区)

    ```bash
    git add <files>
    ```

5. 生成历史版本(提交暂存区内容)

    ```bash
    git commit -m "提交的版本说明"
    ```

6. 生成里程碑(tag)

    ```bash
    # tag查看
    git tag
    git show <tag名称> # 查看指定tag

    # 获取指定远程tag
    git fetch origin tag <远程tag名称>

    # 新建本地tag
    git tag -a <tag名称> -m "tag描述"

    # 推送本地tag 至 远程仓库
    git push origin --tags

    # 删除本地tag
    git tag -d <tag名称>
    # 删除远程tag
    git push origin :refs/tags/<tag名称>

    ```

7. 推送版本变更至远程仓库

    ```bash
    # 命令格式
    git push <远程主机名> <本地分支名>:<远程分支名>
    # 命令格式: 若本地分支与远程分支同名
    git push <远程主机名> <本地分支名>

    # 推送 本地master分支 至 远程master分支
    git push origin master:master
    ```

8. 分支(branch)的基本操作

    ```bash
    # 查看分支
    git branch 或 git branch --list # 列出当前本地分支
    git branch -r # 列出远程分支
    git branch -a # 列出所有本地和远程分支

    # 创建本地分支
    git branch <分支名称>

    # 修改本地分支名称
    git branch -m <已有分支名称> <新分支名称>

    # 删除本地分支
    git branch -d <本地分支名称>
    # 删除远程分支
    git push origin --delete <远程分支名称>

    # 本地分支切换
    git checkout <分支名称>
    # 将远程分支取到本地，并在本地创建分支
    git checkout -b <本地分支名称> origin/<远程分支名称>

    # 将分支(branch-a)合并到当前分支，并自动进行新的提交(commit)和切换HEAD指向
    git merge branch-a

    ```

9. 查看差异:

    ```bash
    # 查看所有变更差异
    git dff

    # 查看指定文件和暂存区的差异
    git diff filename

    # 查看分支之间的差异
    git dff banch-1 branch-2

    # 查看历史版本之间的差异
    git dff commit-id-1 commit-id-2
    ```

### 4.4 .gitignore文件
