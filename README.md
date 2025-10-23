# text-demo
测试git的各个命令

## 分支
现在是在demo分支，我在这里修改一些文件

### PR
现在创建demo0分支
现在提交修改到demo0分支

## Pull Request
我在demo01分支修改了文件内容

## 撤销操作
我在demo01分支修改了文件内容，现在我想撤销这个操作
  - 当文件还在工作区时，就是没有git add 之前 ，可以通过git restore 文件名 来撤销 (有点像 Ctrl + Z)
  - 当文件通过git add 到暂存区时，就可以通过git reset HEAD 文件名 来撤销 或者git restore --staged 文件名
  - 当文件通过git commit 到本地仓库时，需要修改提交的信息就可以通过git commit --amend -m '输入新的提交信息' ,直接来修改，或者git commit --amend 进入编辑器修改
  - 当在git commit 之后，还需要添加新的文件到暂存区时，就可以通过git add 文件名 来添加 
  如果想和之前提交的文件合并到一起，就可以通过git commit --amend --no-edit 来合并（注意：合并时，新添加的文件内容会被合并到之前的提交中）
  也可以通过git commit --amend -m '最终提交' ,合并的同时，重新命名 
  - 在保存到本地仓库之后，可以通过git reset --soft HEAD~1 使得撤销提交，回到暂存区 git reset HEAD~1 直接回到工作区(改变HEAD后面的数字，撤销最近的几次提交)