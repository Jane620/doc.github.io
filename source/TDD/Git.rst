

Git技巧
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

创建本地分支并关联远端仓库分支
git checkout -b branch_name  origin/branch_name

删除本地分支
git branch -D branch_name

关联远端分支到本地分支
git branch --set-upstream-to=origin/branch_name

丢弃本地改动直接更新远端分支到本地
git fetch --all
git reset --hard origin/Venom_eCpri


idle中设置git commit时候检查pylint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 安装pylint : pip install pylint
2. 安装commit-hook : pip install git-pylint-commit-hook
3. 进入项目的.git/hooks下 : mv pre-commit.sample pre-commit
4. 删除pre-commit中内容，并且只保留这两行
    #!/bin/sh
    git-pylint-commit-hook --limit=7.5 --pylintrc=.pylintrc
5. 其中pylintrc文件可以放置在同级目录下，并且自动pylint规则


遇到设置了sshkey依旧需要输入密码
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
有可能repo是在设置sshkey前download下来的，因此可以尝试
1. 另外建一个dir，git clone下来code
2. rm -rf 当前项目下的.git
4. cp -r 新目录的.git  到当前项目下

本地处理merge冲突
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
合并分支branch_A， 被合分支branch_B

git checkout -b branch_A (如果存在，则只要git checkout即可)
git pull
git checkout branch_B
git merge branch_A  (此时提示冲突)
git mergetool (图形显示冲突，点击需要合并的地方，然后save)
git commit -m ""
git push


在gerrit的一次提交中，增加新的patch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
git add files
git commit --amend
git push origin HEAD:refs/for/master (此处为提交gerrit)
