[Learn Git Branching](https://learngitbranching.js.org/?localw=zh_CN) Git主要命令的记录

```bash
#使用^向上移动1个提交记录
#使用~<num>向上移动多个提交记录
# HEAD就像一个指针，指向当前commit的内容

#使用相对应用最多的就是移动分支，可以直接使用 -f选项让分支指向另一个提交
#git checkout移动HEAD节点 
git branch -f main HEAD~3 #会将main分支强制指向HEAD的第3级parent分支

#撤销变更
git reset HEAD~1#对远程分支无效
git revert HEAD#多了一个新的提交，新提交和上一版本提交一致，相当于未改变内容

git cherry-pick <提交号> #把之前的提交放到当前分支下，把提交树上的任何地方的提交记录取过来追加到HEAD上，但是不能是HEAD上游的提交

#交互式rebase, --interactive -i
git rebase -i HEAD~4 #弹出交换式界面
git commit --amend # 替代上一次commit的位置

git tag v0 C1

git describe <ref> #找到最近的tag,返回<tag>_<numCommits>_g<hash>,tag是离ref最近的标签，numCommits表示这个ref与tag相差多少个提交记录，hash是给定的ref所表示的提交记录哈希值的前几位
git checkout HEAD~^2~2 # ^<num>表示合并节点的第num个父节点

# <remoter name>/<branch name> origin/main
git commit 远程分支会出现HEAD分离状态，因为远程分支只能在远程端更新后才会commit

git fetch #从远端仓库下载本地仓库缺失的提交数据，更新远程分支指针，但不会更新本地main指针，也不会修改磁盘上的文件，并没有将本地仓库与远程仓库同步，可能已经将进行这一操作所需的所有数据都下载了下来，但是并没有修改本地文件.fetch不会更新本地的非远程分支
# git pull 就是git fetch和git merge的缩写

git pull --rebase #fetch 和 rebase的简写
pull request
#rebase可以使提交树变得很干净，但是修改了提交树的历史，merge可以保留提交历史
#pull操作时，提交记录会被先下载到o/main上，之后再合并到本地的main分支。
#push操作时，从main推到远程仓库中的main分支，同时更新o/main分支
#main分支指定了推送的目的地以及拉取后合并的目标
git checkout -b totallyNotMain o/main # 创建一个名为totallyNotMain的分支，跟踪远程分支o/main
git branch -u o/main foo #设定foo分支跟踪远程分支
# git push <remote> <place>
git push origin main # 切到本地仓库的“main"分支，获取所有的分支，再到远程仓库“origin”中找到“main"分支，将远程仓库中没有的提交记录都添加上去
# git push origin <source>:<destination>
#git fetch <place>
git fetch origin foo
#git fetch origin <source>:<destination> 不能在当前切换的分支上干这个事儿，只能是其他分支
#git fetch命令不带参数时，会下载所有的提交记录到各个远程分支
#git pull push 不指定任何source 
git push origin:side #push空到远程仓库会删除远程仓库中的分支
git fetch origin:bugFix # fetch空到本地，会创建一个新分支
#git pull 就是fetch和merge的缩写，可以理解为用同样的参数执行git fetch，然后再merge 所抓取到的提交记录
```

