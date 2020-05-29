##### git 重新绑定远程仓库

```
git remote rm origin
git remote add origin 你的新远程仓库地址
```

##### git 拉取远程全部分支

```
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```



##### git 推送本地所有的分支

```
git push --all origin -u
```

##### git 创建基础文件 .git

```github
git init
```

##### git  提交的缓存区

```
git add .
```

##### git 推送到远程分支

```
git push origin master   或者
git push origin 分支名
```

##### git 拉取远程分支

```
git pull
// 可拆分为两步
git fetch origin 分支名 // 远程仓库到本地版本库
git merge origin/分支名 // 版本库到工作区 
// 我比较喜欢git pull --rebase
```

##### git log  查看提交的日志

```
git log 查看提交的记录
git reflog 查看详细日志
git log --graph 图形展示历史记录
```

##### git reset 版本后退

```
git reset --hard 提交记录的版本号 后退回退都可以
```

##### git cherry-pick 跨记录拿提交内容

```
git cherry-pick 版本号 // 根据自己想拿的记录名，拿指定的记录  ，比如你版本后退了，新功能只有一项需要 可以通过这个把那一次的提交拿过来
```



##### git 稍微高级点操作

##### git 分支操作

```
git branch  // 查看当前分支
git branch 分支名
git branch -D 删除分支
git checkout 分支名   // 切换到另一个分支，注意当前分支如果未提交的不能切换
git checkout -b 分支名   // 创建一个分支 并切换过去
git checkout -b 分支名 origin/分支名  // 切换一个分支并拉取远程当前分支的内容
注意： 一般创建切换分支，创建出来的分支是和当前分支内容一样，以创建时所在分支为模板创建
```

##### git 暂存区

```
暂存区使用的条件：
1.当前内容未开发完毕，不想提交代码记录，并且需要切换到其他分支修改其他内容
2.已经开发过的内容，目前又不需要了，后期不确定使用不使用
git stash list // 查询当前暂存区所有记录
git stash save "这里写你提交的内容注释，方便你记忆当时暂存的是什么内容" 
git stash pop // 将你提交的暂存弹出来，默认最后一条，正常弹出后会删除，如果出现冲突，解决完不会删除这条暂存
git stash pop stash @{这里填数字} // 这里指弹出第几次的暂存记录‘
git stash drop stash @{数字} // 这里指丢弃第几次记录
git stash clear // 清空暂存
git show stash@{n} // 查看暂存的具体内容
```

##### git rebase  变基（重点）

```
git rebase 分支名  // 将其他分支合并到当前分支，两个人的提交记录将会合并，可能会冲突，需要解决完继续合并，如果没有冲突就直接合并完成 git log 查看记录就可以了
git add . // 有冲突解决后操作然后执行 ，可能需要执行 git commit --amend 更新提交记录
注意： 如果中间出现无法继续合并可能会有多次冲突，当前冲突解决完不需要 git add. 而是继续执行 git rebase --continue 继续操作
git rebase --continue // 继续执行剩下为合并的
git rebase -- abort // 在未合并完成的时候可以退回到合并前的页面
git push origin 分支名 -f // 强推，如果发生冲突可能会推不上去，需要强推

// git 合并提交记录，让记录更加简洁，注意不是很熟练不要操作！不要操作
git rebase -i 以前提交的记录名   //将现在的提交到以前提交的中间部分一起合并
git rebase -i HEAD~数量   //从当前位置向前合并几次记录
改想合并的那一条前面改成s 然后wq保存，然后&连接
```

##### git merge 合并

```
git merge 分支名 // 合并某个分支到自己分支，和git rebase 不同，这个合并会多出来一条记录，不推荐使用，会使记录分叉不建议使用
git merge --abort  合并出现问题可以后退
```

##### git + 小乌龟让你体验更爽

```
git 下载地址： 
官方： https://git-scm.com/
电脑管家： https://pc.qq.com/search.html#!keyword=git
小乌龟：TortoiseGit
官网： https://tortoisegit.org/download/
电脑管家： https://pc.qq.com/search.html#!keyword=TortoiseGit
中文包也一起下载，如果英文好的可以使用英文
```

##### git 提交记录规范

```
我公司： 
Mod: 修改使用
Fix: 修复使用
Add: 新增使用
Del: 删除使用
// 注意首字母大写，冒号英文后面有一个空格，内容要简洁，提交记录基本上以一个功能或者一个页面提交，中间写英文的话记得单词前后加一个空格
网上的说法：
feat：添加新功能（feature）
fix：改bug
docs： 修改文档（documentation）
style： 只改样式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是改bug的代码变动）
perf : 代码优化（提升代码性能）
test：增加测试
chore：构建过程或辅助工具的变动
```

##### git push 后续操作

```
在git管理库进行pr申请，申请要合并到的分支，一般自己只推自己分支，如果自己的分支没有冲突，主管或者项目组长就会在线上自动合并，如果有冲突，或者代码不规范就会打回来，你需要修改后重新申请合并
如果有冲突，一种是你拉取要合并的分支，然后你合并另外一个分支，冲突解决后再推到你的分支，然后申请pr合并，这样应该就没冲突了，另外一种就是你主管或者组长拉你分支，然后合并后传到主分支，那么在他合并后你记得拉他的分支然后和自己的分支合并，这样你们的记录就合并到一起了，记得用 rebase 合并。
```

