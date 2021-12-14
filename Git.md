## Git命令

```bash
# 设定名字
git config --global  user.name  "xxxx"  
# 设定邮箱
git config --global  user.email  “xxxx”  
# 查看配置
git config --list       
```



```bash
# 将当前目录初始化为可以管理的仓库
git  init
# 把文件添加到仓库（一次可添加多个）
git  add 
# 将文件提交到仓库（【-m “提交说明”】）
git  commit 
# 查看仓库当前状态
git  status 
# 查看修改内容
git  diff 
# 查看提交记录
git  log 
# 回退到上一个版本（【n个^表示回退到前n个版本，或~n】）
git  reset  --hard  HEAD^ 
# 查看命令历史
git  reflog 
```

> 如果已经有A -> B -> C，想回到B：
>
> 方法一：reset到B，丢失C：
>
> A -> B
>
> 方法二：再提交一个**revert**反向修改，变成B：
>
> A -> B -> C -> B
>
> C还在，但是两个B是重复的
>
> 看你的需求，也许C就是瞎提交错了（比如把密码提交上去了），必须reset
>
> 如果C就是修改，现在又要改回来，将来可能再改成C，那你就revert



```bash
# 1.查看所有分支
git branch -a
# 2.查看当前使用分支(结果列表中前面标*号的表示当前使用分支)
git branch
# 3.切换分支
git checkout 分支名
```





## Github使用

#### 1、如何上传本地项目到Github

```bash
git init   # 初始化，在项目目录打开Git Bash，运行此命令，初始化本地仓库
git add .  # 添加文件到仓库，可以跟文件，也可以跟通配符，.表示把当前目录下所有未追踪的文件全部add了
git commit -m "first commit" # 把文件提交到仓库，first commit为提交注释
git branch -m master main  # 将本地的master分支更名为main
git remote add origin git@github.com:user_name/repository_name # 关联远程仓库
git push -u origin main # 将本地main分支的所有内容推送到远程库
```



#### 2、

```bash
 git pull origin master  #拉取远程代码
```

.gitignore文件模板

 https://github.com/github/gitignore/blob/master/Python.gitignore