# **git是什么？**

版本控制系统 version control system

- 本地 vcs
- 集中式 vcs ===>  svn
- 分布式 vsc ===> git

**作用：**代码存档备份，支持多人开发

**四个状态**

跟踪 ===> 工作区 ===> 暂存区 ===> 提交到创库

# 操作命令:

## 查看版本

```bash
git --version
```

## 配置user信息

```bash
// 用户名
git config --global user.name "your name" # user.name后面是空格
// 用户邮箱
git config --global user.email "your email"
```

## 中文乱码

```bash
// 解决git status中文显示问题
git config --global core.quotepath false
```

## 查看用户配置信息

![image-20220819195446756](D:/Typora/imgs/image-20220819195446756.png)

```bash
git config --list  // 列出所有配置 
git config --list --local   // 具体的项目中有配置才生效
git config --list --global  //  --global --list 可以交换位置.
git config --list --system

// 查看所有的配置和它们所在的文件
git config --list --show-origin
```

## 清除 --unset

```
git config --unset --local user.name
git config --unset --global user.name  // 一般用的是global
git config --unset --system user.name
```

## 初始化一个git仓库

```bash
# 方式 1.1   用Git之前已经有项目代码
cd 目录代码所在文件夹
git init

# 方式 1.2   用Git之前还没有项目代码
cd 某个文件夹 
git init project_name // 在在当前路径下创建和项目名称同名的文件夹
cd project_name

# 方式 2    从其他服务器上克隆一个已经存在的Git仓库
cd 文件夹
git clone 某个地址
```

## 检查当前文件状态

```bash
git status 检查当前文件的状态
```

- **红色** 表示工作区中的文件需要提交
- **绿色** 表示暂存区中的文件需要提交

**精简模式**

```bash
git status -s 
git status --short
```

- ?? : 新添加的未跟踪文件
- A : 新添加到暂存区中的文件
- M : 修改过的文件 (暂存区)

## 跟踪新文件

> 使用命令 `git add` 开始跟踪一个文件, 并且添加后的文件处于**暂存状态**

```bash
git add 文件名
```

**向暂存区一次性添加多个文件**

```bash
git add . // 注意 add后面是空格
```

## 提交更新与修改提交描述

```bash
# 1. 提交之前 检查状态  git status
# 2. git commit -m "update"   ===> git commit -m"update" m和消息之间的空格可以省略
# 3. 提交完成后 检测状态  git status
```

```bash
git commit -m "本次提交要记录的提交描述信息"
# 修改最近一次提交说明 amend修正
git commit --amend -m "修改最近一次提交信息"
```

## 跳过使用占存区提交

```bash
git commit -a -m "描述信息"
# 同:
git commit -am "描述信息"
```

## 提交日志

回车， 上下翻页， q退出

```bash
# 按时间先后顺序列出所有的提交历史，最近的提交在最上面
git log

# 只展示最新的两条提交历史，数字可以按需进行填写
git log -2

# 一行显示 ，hash简写
git log --oneline 

# 在一行上展示最近两条提交历史的信息
git log -2 --pretty=oneline

# 在一行上展示最近两条提交历史信息，并自定义输出的格式
# &h 提交的简写哈希值  %an 作者名字  %ar 作者修订日志  %s 提交说明
git log -2 --pretty=format:"%h | %an | %ar | %s"git log
```

## 版本回退

版本回退， 将代码恢复到已经提交的某一个版本中

```bash
# 可以回退到任意版本
git reset --hard 版本号
# 将版本回退到上一次提交
git reset --hard head~1
# 查看所有的版本记录
git reflog -oneline 
```

## 撤销修改(无法恢复-慎用)

```bash
git checkout -- 要撤销修改的文件
```

## 取消暂存的文件

```
git reset HEAD 要移出的文件名称
// git status 再次检查

git reset HEAD . // 空格 英文点号， 把暂存区所有的文件都移到工作区
```

## 删库跑路

```bash
# 从 Git仓库和工作区中同时移除 index.js 文件
git rm -f index.js
# 只从 Git 仓库中移除 index.css，但保留工作区中的 index.css 文件
git rm --cached index.css
```

## 忽略某些文件

**注意:** .gitignore 只忽略没有被追踪的文件,如果文件已经被纳入管理,则修改.gitignore无效

**解决办法:** 先把本地缓存删除,把已暂存 改为 未被追踪的状态.重新提交

```bash
# 1. 清除本地当前的Git缓存
git rm -r --cached .

# 2. 应用.gitignore等本地配置文件重新建立Git索引
git add .

# 3. （可选）提交当前Git版本并备注说明
git commit -m 'update .gitignore'
```

![image-20220820002559849](D:/Typora/imgs/image-20220820002559849.png)

​	**匹配规则**

![image-20220819231622983](D:/Typora/imgs/image-20220819231622983.png)

**glob模式**

![image-20220819231725786](D:/Typora/imgs/image-20220819231725786.png)

**例子**

![image-20220819232211503](D:/Typora/imgs/image-20220819232211503.png)

# git常用命令🔥

------------------

1. 配置user,只需要配置1次
2. git init
3. git status 查看文件状态
4. git add . 将已修改的放到暂存区
5. git commit -m "提交到本地仓库"
6. git log --oneline 查看提交版本记录
7. git reflog 查看所有版本
8. git reset --hard 版本ID

# copy ↓

## 1. 本地推送到远端

### 1.1 HTTPS

```bash
# 1. 本地初始化仓库，
git init 

---
git add .
# 修改提交代码
git commit -m "msg" 有了一次提交版本

##========================同步远端
# 1. 添加远端仓库
git remote add origin https://gitee.com/vrfe/vue-project.git
# 2. 第一次推送
git push -u origin master
```

### 1.2 SSH密钥

#### 生成公钥

- [官网地址](https://gitee.com/help/articles/4181)

```bash
# 1. 生成一对密钥 ， 有公钥和私钥   公钥是带有pub后缀名哪一个
ssh-keygen -t ed25519 -C "xxxxx@xxxxx.com"  

# 2. 查看
cat ~/.ssh/id_ed25519.pub   # cat 表示查看文件内容
# ~ 表示当前用户目录
# 3. 在gitee上配置好 

# 4. 本地检查是否可以用
ssh -T git@gitee.com
# 如果是suceessfully 表示可以了

```

```bash
# 如果想修改remote远端连接 
git remote rm origin 
```

```bash
git remote -v # 查看远端仓库

# 1. 添加本地和远端的关联
git remote add origin git@gitee.com:vrfe/git-new.git

# 2. 第一次推送
git push -u origin master 

# 3. 
# 以后修改推送
# 推送之前， git commit 要提交更新
git push 
```

### 

### 1.3 git clone

```bash
git clone <远端地址> 
# 从远端下载仓库
```

### 1.4 Branch

```bash
# 查看当前分支列表
git branch 
# 基于当前所在分支， 新建一个分支
git branch <分支名>
# 切换分支
git checkout <分支名>
# 创建一个新分支， 并且切换到新分支上去
git checkout -b <新分支名>
```

### 1.5 合并分支

1. 先到新分支上 写功能代码， 并提交 
2. 再切换回要合并的分支， 
3. 合并 git merge 

```bash
# 1. 新建login分支， 并切换到login分支
git checkout -b login
# 2. 写功能代码， 写好后， 提交更新 
git commit -m "login功能代码"
# 3. 把login上的新功能代码， 合并到master分支上
# ！ 注意， 要先切换回master分支
git checkout mater
# 4. 执行合并命令
git merge login
```

### 1.6 删除分支

```bash
git branch -d <分支名>
# 注意不能在自己这个分支上， 删除自己！
```

### 1.7 解决合并冲突

```bash
# 1. 不同的分支 
# 2. 同一个文件
# 3. 不同的修改

# 注意合并之前， 两个分支的代码都需求先提交版本更新！！

git merge start  ==> 产生冲突
# 在代码中 手动解决 ===> 选择一个
# 手动解决完 ==> 需要通过git add . 和 git commit -m "解决冲突"重新提交
git add .  放入暂存区
git commit -m "解决冲突"

```

### 一些常见的命令

```bash
> ： ==> Ctrl + C 退出

# 使用vim编辑器 
# vi 
vim index.html 进入查看文件
修改代码 输入 i 进入insert模式 
这个时候就可以编辑代码了 
编辑完之后 
ESC , 一定要是英文输入法  
# 再输入  shift + :   ==>
==> wq 保存并退出

vi 文件名 编辑文件
i 进入编辑模式
按ESC退出编辑模式
:q  不保存退出
:q! 不保存强制退出
:w  保存
:wq  强制保存并退出


# 
# cat 文件名， 查看文件内容
# mkdir 新建一个文件夹
# touch 新建一个文件
# cd .. 返回上一层   .. 上一层   . 当前目录
# cd 文件夹名 进入文件目录
# pwd 当前文件所在路径
# ls 查看文件列表 （不包含隐藏的）
# ls -a 查看所有文件列表（包含隐藏文件）
# ll 查看列表详情
# 清空控制台 clear    cmd ==> cls 
```

### 本地分支推送到远端仓库

```bash
git push -u 远端仓库别名 本地分支名：远程分支名（如果想推到远端把远端重命名）

# 本地新建一个test分支， 修改， commit； 推送；
git push -u origin test:test-remote
```

### 跟踪分支（检出分支）

- 也就是基于远端的某个分支， 拷贝一份相同的到本地， 可以重命名以下。

```bash
# 从远程仓库中, 把对应的远程分支下载到本地仓库, 保持本地分支和远程分支名称相同
git checkout 远程分支名字

# 示例
git checkout pay

# 如果需要重命名
# 从远程仓库中，把对应的远程分支下载到本地仓库，并把下载的本地分支进行重命名
git checkout -b 本地分支名称 远程仓库名称/远程分支名称

# 示例 相当于本地新建一个分支， 该分支基于远端的某个分支新建
git checkout -b payment origin/pay

```

### 拉取最新代码

```bash
# 拉取远程分支最新代码
git pull 
# 注意 ， 我当前处于哪个分支上
# 就会把远程对应分支， 最新的代码下载并合并到这个分支上。 
```



### 到公司之后的一些操作步骤

1. 公司会给你一个gitlab  , 注册。
2. git clone 将公司的项目代码拉取到本地。
3. 配置密钥 SSH 。。

----

1. 开发功能页面，开发一个弹窗组件 dialog
2. 从远端的master分支，  checkout -b 检出 一个新的分支 ! 一定要从master分支检出 （复制）
3. 在这个分支上修改， 编写功能代码， 最后 这个分支会合并到dev 和 test等分支上 分别进行测试！
4. 最后这个分支的代码没有问题了， 就可以把它合并到master上去， 上线功能代码
