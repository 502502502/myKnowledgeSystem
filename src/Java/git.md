#### Git 常用命令

- `git init`
- `git clone`
- `git remote add origin ***.git`
- `git push -u origin master`
- 推送到远程仓库的dev分支：`git push origin dev`
- `git log`
- `git log --graph --pretty=oneline --abbrev-commit`
- `git status`
- `git diff`
- `git add *`
- `git commit -m "message"`
- commit之后又改了一个小bug，但是又不想增加一个commit，可以用：`git commit --amend --no-edit`，直接将改动添加到上一次的commit中
- `git push`
- `git pull`
- `touch .gitignore`

#### Git 标签管理

- 首先切换到需要打标签的分支上，然后使用`git tag v1.0`就可以在当前commit打上v1.0的标签
- `git tag v1.0 commitID` 对特定commit打标签
- 打标签时加上message：`git tag -a <tagname> -m "message"`
- `git tag` 查看所有标签
- `git show [tagname]` 查看标签详细信息
- `git push origin <tagname>`可以推送一个本地标签到远程仓库
- `git push origin --tags`可以推送全部未推送过的本地标签
- `git tag -d <tagname>`可以删除一个本地标签
- `git push origin :refs/tags/<tagname>`可以删除一个远程标签（先从本地删除）

#### Git 撤销与回滚

- **暂存区**：`git add`之后commit之前存在的区域；**工作区**：`git commit`之后存在的区域；**远程仓库**：`git push`之后；

- 作了修改，但还没`git add`，撤销到上一次提交：`git checkout -f -- filename`；`git checkout -f -- .`

- 作了修改，并且已经

  ```
  git add
  ```

  ，但还没

  ```
  git commit
  ```

  ：

  - 先将暂存区的修改撤销：`git reset HEAD filename`/`git reset HEAD`；此时修改只存在于工作区，变为了 "unstaged changes"；
  - 再利用上面的checkout命令从工作区撤销修改

- `git add`之后，作了修改，想丢弃这次修改：`git checkout -f --filename`会回到最近一次`git add`

- 作了修改，并且已经

  ```
  git commit
  ```

  了，想撤销这次的修改：

  - `git revert commitID`. 其实，`git revert`可以用来撤销任意一次的修改，不一定要是最近一次
  - `git reset --hard commitID`/`git reset --hard HEAD^`（HEAD表示当前版本，几个^表示倒数第几个版本，倒数第100个版本可以用HEAD~100）；参数`--hard`：强制将暂存区和工作区都同步到指定的版本
  - `git reset`和`git revert`的区别是：reset是用来回滚的，将HEAD的指针指向了想要回滚的版本，作为最新的版本，而后面的版本也都没有了；而revert只是用来撤销某一次更改，对之后的更改并没有影响
  - 然后再用`git push -f`提交到远程仓库

#### Git 分支管理

- 创建分支: `git branch test`

- 切换分支: `git checkout test`

- 创建并切换分支：`git checkout -b test`

- 将test分支的更改合并到master分支：先在test分支上commit、push，再：`git checkout master`; `git merge test`

- 如果合并时产生冲突：先手动解决冲突，再合并

- 删除分支：`git branch -d test`

- ```
  git stash
  ```

  - 如果当前分支还有任务没有做完，也不想提交，但此时需要切换或者创建其它分支，就可以使用stash将当前分支的所有修改（包括暂存区）先储藏起来；然后就可以切换到其它分支
  - 在其它分支工作完成之后，首先切换回原来的分支，然后使用`git stash list`命令查看
  - 可以使用`git stash apply <stash number>`恢复之前储藏的工作现场，再使用`git stash drop <stash number>`删除掉储藏的内容
  - 也可以直接用`git stash pop`恢复并删除内容

- 如果在其它分支上做了一个修改（比如修复了一个bug，这次修改有一个commitID），想要将这次修改应用到当前分支上，可以使用：`git cherry-pick commitID`，可以复制一个特定的提交到当前分支