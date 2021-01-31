# 本地git push至github遇到Everything up-to-date的问题

当在本地环境推送代码至GitHub时，有时可能会遇到，没有报错error，执行git push后最后看见提示Everything up-to-date

 但是打开github上并没有看见新推的代码。

其实git提交代码到缓冲区，要push至github上的时候不会将本地所有的分支都推，而此时我们本地环境可能就只有一个master主分支。那么我们就需要新建分支来提交改动，然后合并分支到master。

```
[root@WangtingCentOS wangting837]# git branch
* master
```

解决方式：

```bash
[root@WangtingCentOS wangting837]# git branch newbranch
[root@WangtingCentOS wangting837]# git branch
* master
  newbranch
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# git checkout newbranch
Switched to branch 'newbranch'
[root@WangtingCentOS wangting837]# git branch
  master
* newbranch
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# git add .
[root@WangtingCentOS wangting837]# git commit -m"update"
# On branch newbranch
nothing to commit, working directory clean
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# git status
# On branch newbranch
nothing to commit, working directory clean
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# git merge newbranch
Already up-to-date.
[root@WangtingCentOS wangting837]# git push -u origin master
Username for 'https://github.com': xxxxxxxx@qq.com
Password for 'https://xxxxxxxxx@qq.com@github.com': 
Counting objects: 61, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (57/57), done.
Writing objects: 100% (60/60), 22.32 KiB | 0 bytes/s, done.
Total 60 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To https://github.com/xxxxxxxxxxx/wangting837.git
   a942fb4..87949b2  master -> master
Branch master set up to track remote branch master from origin.
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# git branch -D newbranch
Deleted branch newbranch (was 87949b2).
[root@WangtingCentOS wangting837]# 
[root@WangtingCentOS wangting837]# 
```