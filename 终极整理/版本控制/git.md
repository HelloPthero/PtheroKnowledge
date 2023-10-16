```
https://edu.csdn.net/skill/gml/gml-62c30f9c31f64a1d96af732c47c93f04
```



#### 基本命令

```
git config --global user.name "xx"
git config --global user.email "xxx"
git init  初始化仓库
git status 查看状态
git remote add origin https....关联远程仓库
git clone https...  克隆远程仓库内容 /自动关联
git add .    添加所有文件
git commit -m '备注'  提交文件
git pull origin master 下拉
git push origin master 上传到仓库
git log 查看提交日志
```

#### 分支

##### 本地分支/远程分支/追踪分支

```
本地分支就是我们可以通过git branch查看到的分支，也就是我们自己git仓库所拥有的分支
远程分支是对远程仓库的分支的索引，它其实也是本地分支，只是我们无法移动它，必须要在和中心服务器交互根据服务器更新到本地来的代码移动的，远程分支的作用就是我们上次和中心服务器交互更新得到的最新版本，它也是个指针。
追踪分支比较难理解，它也是一个本地分支，只是它对应了一个远程分支，如果我们本地的某个分支对应了一个特定的远程分支，那么它就是追踪分支，比如我们最初的master分支就是一个追踪分支，它对应远程分支origin/master，这里origin是远程仓库名，当我们在master分支里执行更新(fetch，pull)或是推送(push)，在不指定分支的情况下，默认就是从origin/master分支更新来或者提交到origin/mster分支。
```

##### 命令

```
git branch 查看所有本地分支
git branch branch_name 创建分支
git checkout branch_name 切换到分支
git merge branch_name 将分支中的内容合并到当前分支
```

#### 问题

```
The file will have its original line endings in your working directory
:git config --global core.autocrlf false


```

#### Git在VS中的使用(项目开发)

```
建立分支 提交修改commit 变基 推送 提取 拉取
```

