# 概念篇

## 工具比较

- `svn` : 使用增量备份的方式,来控制版本
- `git` : 使用系统快照的方式,来控制版本

## 相关术语

- `工作区` : gitest 目录
- `本地库` : gitest/.git 目录 (使用`git init`生成)
- `暂存区` : gitest/.git/index 文件

## reset参数对比

1. `--soft` 
   - 仅仅在本地库中,移动指针
2. `--mixed`
   - 仅仅在本地库中,移动指针
   - 重置暂存区
3. `--hard`
   - 仅仅在本地库中,移动指针
   - 重置暂存区
   - 重置工作区

## 哈希算法

1. 什么是哈希算法?
   一种加密算法,将明文转换为密文,是个不可逆的过程!!!
2. 常见的哈希算法?
   - CRC32算法
   - MD5算法
   - SHA-1算法 (git使用的)
3. 哈希算法的特点
   - 不论输入的数据有多大,在使用一种哈希算法的时候,得到的输出结果,永远是固定长度的!
   - 输入的数据确定,加密算法确定,多次得到的输出结果是不变的
   - 输入的数据不定,加密算法确定,多次得到的输出结果是变化的
4. 哈希算法的加密过程,是不可逆的!

# 配置篇

## 配置全局变量

用户名 : `git config --global user.name jeesiye`

邮箱地址 : `git config --global user.email phone@163.com`

中文支持 : `git config --global core.quotepath false`

颜色突显 : `git config --global color.ui true`

提交别名 : `git config --global alias.cm 'commit -m "not commit message"'`

## 删除变量

`git config --global --unset user.name`

## 查询

所有变量 : `git config -l`

指定变量 : `git config uer.name`

## ssh密匙

`ssh-keygen -t rsa -C phone@163.com`

- linux中,在用户目录生成一个隐藏目录`.ssh`
- liunx中,使用的是`openssh`软件的指令
- 在github的settings中,添加ssh密匙,切记是公共密匙!

# 本地库

## 帮助指令

`git help status`

`git status --help`

## 最常用指令

初始化 : `git init`

添加暂存区 : `git add ./` 	`git add --all`

提交本地库 : `git commit -m 'not commit'`

## 日志

哈希码,日期,提交者等 : `git log`

哈希码,回退步骤,等 : `git reflog`

长哈希码,提交信息 : `git log --pretty=oneline`

短哈希码,提交信息 : `git log --oneline`

## 回退

任意版本 : `git reset --hard 前7位哈希码值`

上1步 : `git reset --hard HEAD^`

上3步 : `git reset --hard HEAD^^^`

上100步 : `git reset --hard HEAD~100`

## 其他

当前状态 : `git status`

文件比较 : `git diff HEAD^ 1.txt`

## 分支管理

### 查询

本地库分支 : `git branch`

本地库分支详细信息 : `git branch -v`

远程库分支 : `git branch -r`

本地库和远程库分支 : `git branch -a`

### 基础操作

创建分支 : `git branch dev`

删除分支 : `git branch -d dev`

切换分支 : `git checkout dev`

合并分支 : `git merge dev` (提前切换到合并的目标分支上)  (merge 陌截)

远程库分支关联到本地分支 : `git checkout --track origin/master` (--track 等价 -t)

> 分支冲突的解决办法
>
> `<<<` 指示的是当前的分支
>
> `>>>` 指示的是被合并的分支
>
> 修改好冲突的文件后,需要执行以下步骤
>
> `git add --all`
>
> `git cm`
>
> `git merge dev`

# 远程库

## 关联

输出别名简要信息 : `git remote`

输出别名冗余信息 : `git remote -v`

关联远程库 : `git remote add origin git@github.com:jeesiye/gitest.git` (默认的别称是origin)

更改别名 : `git remote rename origin gitest`

删除关联 : `git remote remove origin`(删除后,`git push`命令就执行失败)

## 拉取

拉取所有的分支 : `git fetch origin`

拉取指定的分支 : `git fetch origin master`

## 拉取并合并

拉取远程库,并合并本地库 : `git pull origin` (pull = fetch + merge)

## 推送

推送所有分支 : `git push origin --all`

推送指定分支 : `git push origin master` 

## 克隆

克隆远程库默认的主分支,使用默认别名 : `git clone git@github.com:jeesiye/gitest.git`

克隆远程库的指定分支 : `git clone -b dev git@github.com:jeesiye/gitest.git`

克隆远程库默认分支,指定别名 : `git clone -o gitest git@github.com:jeesiye/gitest.git` 

## 拉取远程库所有分支的步骤

```shell
mkdir gitest
cd gitest
git init
git remote add origin git@github.com:jeesiye/gitest.git
git fetch origin
git checkout --track origin/master
git checkout -t origin/dev
git checkout -t origin/pro
git branch
git branch -r
```

