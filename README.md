# Git

<a href='https://www.bilibili.com/video/BV1WW411Q7EW/'>教学视频</a>

---

[TOC]

---

## 1. `git-help`：帮助手册

```shell
git help  # 帮助
git help -a  # 全部命令
git help -g  # 使用手册
git help [命令]  # 查看命令说明，F向下翻页，B向前翻页，Q退出
```

## 2.`git-config`：配置

```shell
# 配置一共有三个范围：system, global(用户), project(项目)
# 一般配置在 global
git config --global [配置选项]
git config --list  # 查看配置
git config --unset --global [配置选项]  # 重置选项
git config --global color.ui true  # 添加输出色彩

# 配置信息处于用户家目录的 .gitconfig
cat ~/.gitconfig  # 查看配置文件
```

## 3.`git-init`：初始化项目

```shell
# 新建项目文件夹并进入
mkdir gitLearn
cd gitLearn
# 初始化一个空白的git repository
git init
$ Initialized empty Git repository in /Users/iyoc/ProjectFiles/GitRepos/gitLearn/.git/
# 之后 git 就会跟踪这个目录中的文件变化

# 调查一下 .git 目录中的文件（隐藏文件可以使用 open 命令来在 finder 中打开目录）
$ .rw-r--r-- 137 iyoc  1 12 17:52 config  # 项目的 git 配置
$ .rw-r--r--  73 iyoc  1 12 17:52 description
$ .rw-r--r--  21 iyoc  1 12 17:52 HEAD
$ drwxr-xr-x   - iyoc  1 12 17:52 hooks
$ drwxr-xr-x   - iyoc  1 12 17:52 info
$ drwxr-xr-x   - iyoc  1 12 17:52 objects
$ drwxr-xr-x   - iyoc  1 12 17:52 refs
# 以上文件有各自的功能，通常情况下无需自行更改
```

## 4.`git-commit`：提交

查看当前`git`状态

```shell
git status
# 显示以下信息
$ On branch main  # 处于主分支
$
$ No commits yet  # 没有任何提交
$
$ nothing to commit (create/copy files and use "git add" to track)
```

没有文件，创建一个：

```shell
touch initial.py
$ drwxr-xr-x - iyoc  1 12 18:49 .git
$ .rw-r--r-- 0 iyoc  1 12 18:52 initial.py
```

再次查看`git`状态：

```shell
$ On branch main
$ 
$ No commits yet
$ 
$ Untracked files:  # 未跟踪的文件
$  (use "git add <file>..." to include in what will be committed)
$ 	initial.py
$
$ nothing added to commit but untracked files present (use "git add" to track)
```

显示有Untracked files，那么可以使用 `git add`命令进行添加修改后的文件：

```shell
git add initial.py  # 添加指定文件
git add .  # 添加所有文件
```

再次查看`git`状态，提示已经添加了刚才`add`的文件：

```shell
$ On branch main
$
$ No commits yet
$
$ Changes to be committed:  # 将要被提交的改动
$   (use "git rm --cached <file>..." to unstage)
$ 	new file:   initial.py
```

修改完成即可提交，提交时需要指定一个提交的描述信息，使用参数`-m`，此信息有用且重要，尽量描述清除本次提交所涉及到的行为。如果没有添加`-m`参数，`git`会打开默认的文本编辑器让使用者添加描述。

```shell
git commit -m '添加 initial.py 文件'
# 效果如下
$ [main (root-commit) bbe98ba] 添加 initial.py 文件
$  1 file changed, 2 insertions(+)
$  create mode 100644 initial.py
```

提交完成后再次查看一下状态：

```shell
$ On branch main
$ nothing to commit, working tree clean  # 没有可提交的文件，工作区是干净的
```

`git log`以查看以往的`commit`：

```shell
$ commit bbe98ba4972adfe32e9e85b0b1a8d267c3d15d55 (HEAD -> main)  # 提交id
$ Author: yoc <yyyyyoc@hotmail.com>  # 提交人
$ Date:   Thu Dec 1 19:04:25 2022 +0800  # 提交时间
$
$     添加 initial.py 文件  # 提交时的描述信息
```

因为只做过一次提交，所以只显示一条信息。

## 5.`git-diff`：查看修改前后的对比

修改`initial.py`文件的内容之后查看下状态：

```shell
$ On branch main
$ Changes not staged for commit:  # 有未提交的修改，但还未将其放置在暂存区中
$   (use "git add <file>..." to update what will be committed)
$   (use "git restore <file>..." to discard changes in working directory)
$ 	modified:   initial.py  # 修改的文件
$
$ no changes added to commit (use "git add" and/or "git commit -a")
```

查看修改前后的区别（仓库中的文件和工作目录中文件的区别）：

```shell
git diff initial.py

# 效果如下
$ diff --git a/initial.py b/initial.py
$ index 821def7..919bd48 100644
$ --- a/initial.py
$ +++ b/initial.py
$ @@ -1,2 +1,2 @@
$  if __name__ == "__main__":
$ -    print("First Commit!")  # 移除了
$ +    print("Committed!")  # 添加了
```

将修改过后的文件添加到暂存区并查看状态：

```shell
git add initial.py
git status

$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	modified:   initial.py
```

这时，再次查看`diff`时由于没有任何区别，所以不显示任何信息。

如果想要对比仓库和暂存区的文件对比，使用`--staged`参数：

```shell
git diff --staged

# 效果如下
$ diff --git a/initial.py b/initial.py
$ index 821def7..919bd48 100644
$ --- a/initial.py
$ +++ b/initial.py
$ @@ -1,2 +1,2 @@
$  if __name__ == "__main__":
$ -    print("First Commit!")
$ +    print("Committed!")
```

## 6.`git-rename`：重命名已跟踪的文件

添加一个新文件`unameed.py`，保存并提交，然后查看状态。

```shell
# 提交
$ [main aa5c243] 添加了 unamed.py 文件
$  1 file changed, 4 insertions(+)
$  create mode 100644 unamed.py

# 状态
$ On branch main
$ nothing to commit, working tree clean  # 工作区干净
```

如果是在文件系统中将文件重命名，查看状态时，`git`只知道是删除了原文件并新建了一个新文件：

```shell
$ On branch main
$ Changes not staged for commit:
$   (use "git add/rm <file>..." to update what will be committed)
$   (use "git restore <file>..." to discard changes in working directory)
$ 	deleted:    unamed.py
$
$ Untracked files:
$   (use "git add <file>..." to include in what will be committed)
$ 	renamed.py
$
$ no changes added to commit (use "git add" and/or "git commit -a")
```

这事，就需要将`git`中的文件进行删除并重新添加重命名后的文件：

```shell
git rm unamed.py
$ rm 'unamed.py'
git add renamed.py
```

查看状态，提交：

```shell
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	renamed:    unamed.py -> renamed.py  # 这里可以看到是重命名

git commit -m '将 unamed.py 重命名为了 renamed.py'
$ [main a27aabc] 将 unamed.py 重命名为了 renamed.py
$  1 file changed, 0 insertions(+), 0 deletions(-)
$  rename unamed.py => renamed.py (100%)
```

## 7.`git-mv`：移动/重命名文件或目录

### 7.1.重命名

```shell
git mv renamed.py mved.py
git status
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	renamed:    renamed.py -> mved.py
git commit -m '使用 mv 进行重命名'
$ [main f03d6e3] 使用 mv 进行重命名
$  1 file changed, 0 insertions(+), 0 deletions(-)
$  rename renamed.py => mved.py (100%)
```

### 7.2.移动

新建文件夹`src`：

```shell
mkdir src
la
$ drwxr-xr-x  - iyoc  1 12 19:47 .git
$ .rw-r--r-- 51 iyoc  1 12 19:26 initial.py
$ .rw-r--r-- 55 iyoc  1 12 19:34 mved.py
$ drwxr-xr-x  - iyoc  1 12 19:49 src
```

查看状态，发现工作区干净，并没有显示加入了新目录：

```shell
git status
$ On branch main
$ nothing to commit, working tree clean
```

这是因为`git`只会跟踪文件而不是目录，除非该目录中包含文件。接着，使用`mv`将`mved.py`移动到`src`目录中：

```shell
git mv mved.py src/
git status
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	renamed:    mved.py -> src/mved.py
git commit -m '将 mved.py 移动到了 src/ 目录下'
$ [main 6e08835] 将 mved.py 移动到了 src/ 目录下
$  1 file changed, 0 insertions(+), 0 deletions(-)
$  rename mved.py => src/mved.py (100%)
```

## 8.`git-rm`：删除已经跟踪的文件

两种方法：

### 8.1.先在文件系统中删除，然后再使用`git rm`在`git`中删除

```shell
略
```

### 8.2.直接使用`git rm`进行删除

在使用`git rm`命令之前，要确定要删除的文件已经在仓库中，并且没有将要被提交的修改。也就是说，如果修改了这个文件但还未提交，`git`并不会删除这个文件，需要添加并提交后再进行删除才可完成删除。

删除`mved.py`文件：

```shell
git rm src/mved.py
$ rm 'src/mved.py'
git status
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	deleted:    src/mved.py
git commit -m '删除了 mved.py 文件'
$ [main c2c218f] 删除了 mved.py 文件
$  1 file changed, 4 deletions(-)
$  delete mode 100644 src/mved.py
```

## 9.`git-head`：最近次提交的快照（可用来恢复文件）

删除`initial.py`文件并将其恢复：

```shell
git rm initial.py
$ rm 'initial.py'
git status
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	deleted:    initial.py
la
$

# 想要恢复：git checkout [某次提交] [分支] [文件名]
git checkout HEAD -- initial.py  # 将initial.py恢复到最近的一次提交的状态
la
$ drwxr-xr-x  - iyoc  1 12 20:16 .git
$ .rw-r--r-- 51 iyoc  1 12 20:16 initial.py

git status
$ On branch main
$ nothing to commit, working tree clean
```

删除了文件并且提交之后也是可以使用这个命令把文件恢复回来：

```shell
git rm initial.py
$ rm 'initial.py'
git commit -m '删除 initial.py 文件'
$ [main 700a617] 删除 initial.py 文件
$  1 file changed, 2 deletions(-)
$  delete mode 100644 initial.py
la
$
git status
$ On branch main
$ nothing to commit, working tree clean
git checkout HEAD^ -- initial.py  # HEAD^表示最近的一次提交的上一次提交，几个表示上几次
la
$ drwxr-xr-x  - iyoc  1 12 20:22 .git
$ .rw-r--r-- 51 iyoc  1 12 20:22 initial.py
git status
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	new file:   initial.py
git commit -m '恢复了 initial.py 文件'
$ [main c1a4a83] 恢复了 initial.py 文件
$  1 file changed, 2 insertions(+)
$  create mode 100644 initial.py
```

不仅可以恢复被删除的文件，如果对文件进行了修改想要恢复，同样可以使用这样的方法。

## 10.`git-revert`：恢复提交

添加`src/main.py`并提交：

```shell
git status
$ On branch main
$ Untracked files:
$   (use "git add <file>..." to include in what will be committed)
$ 	src/
$
$ nothing added to commit but untracked files present (use "git add" to track)
git add .
git commit -m '添加了 src/main.py'
$ [main 28a86cf] 添加了 src/main.py
$  1 file changed, 3 insertions(+)
$  create mode 100644 src/main.py
```

添加`templates/index.html`并提交：

```shell
git status
$ On branch main
$ Untracked files:
$   (use "git add <file>..." to include in what will be committed)
$ 	templates/
$
$ nothing added to commit but untracked files present (use "git add" to track)
git add .
git commit -m '添加了 templates/index.html'
$ [main 3f62d42] 添加了 templates/index.html
$  1 file changed, 1 insertion(+)
$  create mode 100644 templates/index.html
```

查看提交历史（使用`--oneline`以单行形式查看历史）：

```shell
git log --oneline
$ 3f62d42 (HEAD -> main) 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ c1a4a83 恢复了 initial.py 文件
$ 700a617 删除 initial.py 文件
$ c2c218f 删除了 mved.py 文件
$ 6e08835 将 mved.py 移动到了 src/ 目录下
$ f03d6e3 使用 mv 进行重命名
$ a27aabc 将 unamed.py 重命名为了 renamed.py
$ aa5c243 添加了 unamed.py 文件
$ 3eae325 更改了 initial.py 文件
$ bbe98ba 添加 initial.py 文件
```

恢复`src/main`的提交的话，需要使用其`commiti id`=28a86cf：

```shell
git revert 28a86cf
```

之后会跳转至默认编辑器进行关于此次恢复描述信息的撰写：

```shell
Revert "添加了 src/main.py"

This reverts commit 28a86cf5d255b7187598a79ae8cf362a9e875d5d.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch main
# Changes to be committed:
#       deleted:    src/main.py
#
```

然后恢复完成，查看日志：

```shell
$ [main b059224] Revert "添加了 src/main.py"
$  1 file changed, 3 deletions(-)
$  delete mode 100644 src/main.py

git log --oneline
$ b059224 (HEAD -> main) Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ c1a4a83 恢复了 initial.py 文件
$ 700a617 删除 initial.py 文件
$ c2c218f 删除了 mved.py 文件
$ 6e08835 将 mved.py 移动到了 src/ 目录下
$ f03d6e3 使用 mv 进行重命名
$ a27aabc 将 unamed.py 重命名为了 renamed.py
$ aa5c243 添加了 unamed.py 文件
$ 3eae325 更改了 initial.py 文件
$ bbe98ba 添加 initial.py 文件
```

这是再查看工作目录，发现`src/main.py`的这次提交就已经不见了。

## 11.`git-reset`：重置`commit`

在默认提交以后，`HEAD`指针都会指向最近的一次提交。使用`git reset`可以去控制这个指针的位置。重置commit时，共有有三种参数选项：

- `--soft`：软重置，重置commit时不会影响工作区和暂存区的文件的当前状态。
- `--hard`：硬重置，重置commit时会将工作区和暂存区的文件重置到指定commit的状态。
- `--mixed`：（默认）重置commit时会将暂存区的文件重置到指定commit的状态，并将HEAD指向该次commit。

先查看下最近的几个commit：

```shell
$ b059224 (HEAD -> main) Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
```

使用软重置将其恢复到`3f62d42`时的状态：

```shell
git reset --soft 3f62d42
git log --oneline
$ 3f62d42 (HEAD -> main) 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ c1a4a83 恢复了 initial.py 文件
$ 700a617 删除 initial.py 文件
$ c2c218f 删除了 mved.py 文件
$ 6e08835 将 mved.py 移动到了 src/ 目录下
$ f03d6e3 使用 mv 进行重命名
$ a27aabc 将 unamed.py 重命名为了 renamed.py
$ aa5c243 添加了 unamed.py 文件
$ 3eae325 更改了 initial.py 文件
$ bbe98ba 添加 initial.py 文件
```

此时`HEAD`指针就会指向`3f62d42`，查看当前状态：

```shell
$ On branch main
$ Changes to be committed:
$   (use "git restore --staged <file>..." to unstage)
$ 	deleted:    src/main.py
```

如果此时commit，那么就会覆盖掉`b059224`。

接下来试用`--mixed`进行重置：

```shell
git reset --mixed 3f62d42
$ Unstaged changes after reset:
$ D	src/main.py
git status
$ On branch main
$ Changes not staged for commit:
$   (use "git add/rm <file>..." to update what will be committed)
$   (use "git restore <file>..." to discard changes in working directory)
$ 	deleted:    src/main.py
$
$ no changes added to commit (use "git add" and/or "git commit -a")
```

此时，暂存区中的文件不复存在，如果需要重新添加，需要使用`git-add`重新将工作区中的文件添加到暂存区。

最后使用`--hard`将其重置：

```shell
git reset --hard 3f62d42
$ HEAD is now at 3f62d42 添加了 templates/index.html
git status
$ On branch main
$ nothing to commit, working tree clean
```

此时，不仅暂存区，工作区内的文件也被重置。由于未进行提交，此时也可恢复至初始状态`b059224`：

```shell
git reset --hard b059224
$ HEAD is now at b059224 Revert "添加了 src/main.py"
```

## 12.`git-branch`：分支结构

在进行新功能的编写和Debug时，可以先开辟新分支，在新分支上确定修改完成后再讲新分支和旧分支合并。

首先查看状态，确定处于哪个分支：

```shell
git status
$ On branch main
$ nothing to commit, working tree clean
```

可以看到，当前处于主分支`main`上，查看该项目的所有分支：

```shell
git branch
$ * main
```

创建分支`git branch [分支名称]`：

```shell
git branch dl-feature
git branch
$   dl-feature
$ * main
```

切换分支`git checkout [分支名]`：

```shell
git checkout dl-feature
$ Switched to branch 'dl-feature'
git branch
$ * dl-feature
$   main
```

## 13.`git-checkout`：切换分支

在`dl-feature`分支上的`initial.py`中添加代码后添加暂存并提交：

```shell
git add .
git commit -m '添加了 torch'
$ [dl-feature 1ff6eb4] 添加了 torch
$  1 file changed, 3 insertions(+)
```

查看详细的log状态：

```shell
git log --oneline --decorate
$ 1ff6eb4 (HEAD -> dl-feature) 添加了 torch
$ b059224 (main) Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ c1a4a83 恢复了 initial.py 文件
$ 700a617 删除 initial.py 文件
$ c2c218f 删除了 mved.py 文件
$ 6e08835 将 mved.py 移动到了 src/ 目录下
$ f03d6e3 使用 mv 进行重命名
$ a27aabc 将 unamed.py 重命名为了 renamed.py
$ aa5c243 添加了 unamed.py 文件
$ 3eae325 更改了 initial.py 文件
$ bbe98ba 添加 initial.py 文件
```

可以看到`HEAD`指向了`dl-feature`分支。此时，如果切换到`main`分支时，`dl-feature`分支上修改的内容并未影响`main`分支。

查看状态：

```shell
git checkout main
$ Switched to branch 'main'
git log --oneline --decorate
$ b059224 (HEAD -> main) Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ c1a4a83 恢复了 initial.py 文件
$ 700a617 删除 initial.py 文件
$ c2c218f 删除了 mved.py 文件
$ 6e08835 将 mved.py 移动到了 src/ 目录下
$ f03d6e3 使用 mv 进行重命名
$ a27aabc 将 unamed.py 重命名为了 renamed.py
$ aa5c243 添加了 unamed.py 文件
$ 3eae325 更改了 initial.py 文件
$ bbe98ba 添加 initial.py 文件
```

可以看到，此时`HEAD`指针指向了`b059224`。

想要查看所有分支的提交：

```shell
git log --oneline --decorate --all
$ 1ff6eb4 (dl-feature) 添加了 torch
$ b059224 (HEAD -> main) Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ c1a4a83 恢复了 initial.py 文件
$ 700a617 删除 initial.py 文件
$ c2c218f 删除了 mved.py 文件
$ 6e08835 将 mved.py 移动到了 src/ 目录下
$ f03d6e3 使用 mv 进行重命名
$ a27aabc 将 unamed.py 重命名为了 renamed.py
$ aa5c243 添加了 unamed.py 文件
$ 3eae325 更改了 initial.py 文件
$ bbe98ba 添加 initial.py 文件
```

## 14.`git-branch-diff`：分支区别

对比两个分支：

```shell
# git diff [分支1]..[分支2]
git diff main..dl-feature
$ diff --git a/initial.py b/initial.py
$ index 919bd48..808aea6 100644
$ --- a/initial.py
$ +++ b/initial.py
$ @@ -1,2 +1,5 @@
$ +import torch
$ +
$ +
$  if __name__ == "__main__":
$      print("Committed!")
```

对比两个分支中的具体文件：

```shell
# git diff [分支1]..[分支2] [文件名]
git diff main..dl-feature initial.py
$ diff --git a/initial.py b/initial.py
$ index 919bd48..808aea6 100644
$ --- a/initial.py
$ +++ b/initial.py
$ @@ -1,2 +1,5 @@
$ +import torch
$ +
$ +
$  if __name__ == "__main__":
$      print("Committed!")
```

## 15.`git-fast-forward`：一种合并的类型

首先保证当前处于`main`分支，将`dl-feature`分支合并进来：

```shell
git merge dl-feature
$ Updating b059224..1ff6eb4
$ Fast-forward  # 是fast-forward类型的合并
$  initial.py | 3 +++
$  1 file changed, 3 insertions(+)
```

从上面可以看到是一个`Fast-forward`类型的合并：这是由于在创建了新分支后，旧分支没有进行任何的更改，所以在合并时，可以直接将新分支上的修改添加进来，这个合并其实并不会是一个新的commit，查看日志：

```shell
git log --oneline --decorate
$ 1ff6eb4 (HEAD -> main, dl-feature) 添加了 torch
$ b059224 Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ 28a86cf 添加了 src/main.py
$ ···
```

可以看到，`HEAD`指针此时指向了`dl-feature`上的最后一次commit。此时，由于已经合并，查看分支区别时不会显示任何区别：

```shell
git diff main..dl-feature
$ 
```

## 16.`git-merge`：分支合并

如果在开辟了新分支并对新分支进行了修改和commit，原有分支也进行了修改和commit，那么此时若要将分支进行合并的话，就是一次真正的合并。

先切换到`dl-feature`分支并查看分支状态：

```shell
git checkout dl-feature
$ Switched to branch 'dl-feature'
git branch
$ * dl-feature
$   main
```

修改该分支上的`initial.py`文件并提交修改：

```shell
# 使用am参数进行添加和提交
git commit -am '添加了 torchvision'
$ [dl-feature f889e19] 添加了 torchvision
$  1 file changed, 1 insertion(+)
git log --oneline
$ f889e19 (HEAD -> dl-feature) 添加了 torchvision
$ 1ff6eb4 (main) 添加了 torch
$ b059224 Revert "添加了 src/main.py"
$ 3f62d42 添加了 templates/index.html
$ ···
```

然后切换到主分支并添加修改并提交：

```shell
git checkout main
$ Switched to branch 'main'
touch utils.py
git status
$ On branch main
$ Untracked files:
$   (use "git add <file>..." to include in what will be committed)
$ 	utils.py
$
$ nothing added to commit but untracked files present (use "git add" to track)
git add .
git commit -m '添加了 utils.py'
$ [main 544a6e1] 添加了 utils.py
$  1 file changed, 4 insertions(+)
$  create mode 100644 utils.py
```

接下来查看所有分支的所有commit：

```shell
git log --oneline --decorate --all -5 --graph  # all-所有，5-五条，graph-图形显示
$ * 544a6e1 (HEAD -> main) 添加了 utils.py
$ | * f889e19 (dl-feature) 添加了 torchvision
$ |/
$ * 1ff6eb4 添加了 torch
$ * b059224 Revert "添加了 src/main.py"
$ * 3f62d42 添加了 templates/index.html
```

这时就满足开始时提到的情况了，想要进行合并就不能再进行`fast-forward`合并了，需要用新的合并提交方式：

```shell
git branch
$   dl-feature
$ * main
git merge dl-feature
# 然后会进入默认的文本编辑器进行描述的填写。这里使用默认的描述。
$ Merge made by the 'ort' strategy.
$  initial.py | 1 +
$  1 file changed, 1 insertion(+)
```

然后再查看日志，会发现有一个新的commit：

```shell
git log --oneline --decorate --all -5 --graph  # all-所有，5-五条，graph-图形显示
$ *   cc2cfc0 (HEAD -> main) Merge branch 'dl-feature'
$ |\
$ | * f889e19 (dl-feature) 添加了 torchvision
$ * | 544a6e1 添加了 utils.py
$ |/
$ * 1ff6eb4 添加了 torch
$ * b059224 Revert "添加了 src/main.py"
```

## 17.`git-conflict`：冲突解决

切换到`main`分支并修改`initial.py`的内容，提交然后切换到`dl-feature`分支：

```shell
git checkout main
$ Already on 'main'
vim initial.py
git commit -am '修改了一些 initial.py 的内容'
$ [main 8fadeb5] 修改了一些 initial.py 的内容
$  1 file changed, 1 insertion(+), 1 deletion(-)
```

然后对`dl-featue`分支中的同一条内容进行修改：

```shell
git checkout dl-feature
$ Switched to branch 'dl-feature'
vim initial.py
git commit -am '也修改了一些 initial.py 的内容'
$ [dl-feature ce98262] 也修改了一些 initial.py 的内容
$  1 file changed, 1 insertion(+), 1 deletion(-)
```

查看状态：

```shell
git log --oneline --decorate --all -5 --graph  # all-所有，5-五条，graph-图形显示
$ * ce98262 (HEAD -> dl-feature) 也修改了一些 initial.py 的内容
$ | * 8fadeb5 (main) 修改了一些 initial.py 的内容
$ | *   cc2cfc0 Merge branch 'dl-feature'
$ | |\
$ | |/
$ |/|
$ * | f889e19 添加了 torchvision
$ | * 544a6e1 添加了 utils.py
$ |/
```

将`main`分支合并到当前分支`dl-feature`上：

```shell
git merge main
$ Auto-merging initial.py
$ CONFLICT (content): Merge conflict in initial.py
$ Automatic merge failed; fix conflicts and then commit the result.
```

此时进行合并，两个不同分支中的同一个文件的同一行内容不一致，有冲突。

- 若想要放弃本次合并，可以使用`git merge abort`命令。

- 手动解决冲突——查看该冲突文件内容，可以发现git已经标注出了冲突的位置和内容：

```shell
cat initial.py
$ ···
$ if __name__ == "__main__":
$ <<<<<<< HEAD
$     print("dl-feature!")
$ =======
$     print("main!")
$ >>>>>>> main
```

此时，删除不要的内容、保留需要的内容之后进行add和commit即可：

```shell
git add .
git commit
# 这时可以不用为commit添加描述，git 会自动添加一个描述。在这个文本编辑器中删除刚刚的冲突信息。
$ [dl-feature 684b72d] Merge branch 'main' into dl-feature
```

这样就完成了这次有冲突的合并。

## 18.`rm-branch`：删除分支

创建新分支：

```shell
git branch bugfix
git branch --list
$   bugfix
$ * dl-feature
$   main
```

重命名新分支：

```shell
git branch -m bugfix debug
git branch --list
$   debug
$ * dl-feature
$   main
```

删除分支：

```shell
git branch -d debug
$ Deleted branch debug (was 684b72d).
```

## 19.`stash`：将修改暂存在指定位置

创建`requirements.txt`，commit后修改其内容并查看状态：

```shell
git status
$ On branch dl-feature
$ Untracked files:
$   (use "git add <file>..." to include in what will be committed)
$ 	requirements.txt
$
$ nothing added to commit but untracked files present (use "git add" to track)
```

如果这是想要去修改其他文件但又不想和`requirements.txt`这个修改一同commit，那么可以使用`stash`来保存当前的工作进度：

```shell
git stash save '添加依赖需求'
$ Saved working directory and index state On dl-feature: 添加依赖需求
```

执行完该命令之后，`requirements.txt`文件又会恢复到未修改之前的状态，查看当前状态：

```shell
$ On branch dl-feature
$ nothing to commit, working tree clean
```

如果想要查看保存的工作状态：

```shell
git stash list
$ stash@{0}: On dl-feature: 添加依赖需求
git stash show -p stash@{0}  # p-以补丁的方式检查区别
$ diff --git a/requirements.txt b/requirements.txt
$ index 8b13789..12c6d5d 100644
$ --- a/requirements.txt
$ +++ b/requirements.txt
$ @@ -1 +1 @@
$ -
$ +torch
```

恢复工作进度：

```shell
git stash apply stash@{0}
$ On branch dl-feature
$ Changes not staged for commit:
$   (use "git add <file>..." to update what will be committed)
$   (use "git restore <file>..." to discard changes in working directory)
$ 	modified:   requirements.txt
$
$ no changes added to commit (use "git add" and/or "git commit -a")
```

删除工作进度：

```shell
git stash drop stash@{0}
$ Dropped stash@{0} (267e049194b22081d8e296fcdca8464488af6f5a)
git stash list
$
```

当然也可以在恢复工作进度时直接删除：

```shell
git stash pop stash@{0}
```

## 20.`log`：查看提交日志

- `F`向下翻页、`B`向上翻页、`Q`退出。

- `--oneline`：单行显示

- `-num`：显示num行

- `--author="作者"`：显示作者的commit

- `--grep="内容"`：显示描述中包含内容的commit

- `--before="日期"`：显示日期之前的commit（YYYY-mm-dd）

  > 也可以按照日期数`--before="1 week/3 days"`

- `--graph`：图形状态显示

也可以使用`git help log`查看帮助信息。

## 21.`alias`：给常用命令添加别名

```shell
# git config --global alias.[别名] [命令]
git config --global alias.co checkout
cat ~/.gitconfig
$ [alias]
$ 	co = checkout
```

## 22.`ignore`：忽略跟踪

全局忽略跟踪：

```shell
git config --global core.excludesfile ~/.gitignore_global
```

查看效果——在工作区中添加`.DS_Store`文件，然后查看状态：

```shell
touch .DS_Store
la
$ .rw-r--r--  0 iyoc  2 12 17:03 .DS_Store
$ drwxr-xr-x  - iyoc  2 12 16:58 .git
$ .rw-r--r-- 86 iyoc  2 12 15:55 initial.py
$ .rw-r--r--  6 iyoc  2 12 16:39 requirements.txt
$ drwxr-xr-x  - iyoc  1 12 20:37 templates
$ .rw-r--r-- 54 iyoc  2 12 15:49 utils.py
git status
$ On branch dl-feature
$ Changes not staged for commit:
$   (use "git add <file>..." to update what will be committed)
$   (use "git restore <file>..." to discard changes in working directory)
$ 	modified:   requirements.txt
$
$ no changes added to commit (use "git add" and/or "git commit -a")
```

并没有这个文件！

另，还有一些可以忽略掉的文件：<a href='gist.github.com/octocat/9257657'>列表</a>

## 23.`gitignore`：为每个项目创建单独的忽略列表

```shell
vim .gitignore
```

`git`不会忽略已经跟踪的文件，如果想要删除，使用`rm`进行删除。

可以根据<a href="github.com/github/gitignore">模板</a>对不同的项目进行文件忽略。

## 24.`remote`：在远程服务器上创建Repository并将本地repo向上推送

可以将他人的commit`fetch`到本地，然后再将其`merge`到项目中。

## 25.`origin`：远程Repository（惯例）

添加到远程仓库：

```shell
git remote add origin git@github.com:YOCdot/gitLearn.git

git remote  # 查看远程（简略信息）
$ origin
git remote -v  # 查看远程（详细信息，v-verbose）
$ origin	git@github.com:YOCdot/gitLearn.git (fetch)  # 拉取
$ origin	git@github.com:YOCdot/gitLearn.git (push)  # 推送
# 这两个地址默认是同一个地址
# 如果想要移除这个远程origin的话：git remote rm [远程库的名字]

git branch -M main  # -M：即使新的分支名称已经存在，也要移动/重命名分支。

git push -u origin main
```

## 26.`push`：推送

```shell
# git push [选项] [远程] [分支]
# -u(set up stream)：跟踪远程分支的变化
git push -u origin main
$ Enumerating objects: 44, done.
$ Counting objects: 100% (44/44), done.
$ Delta compression using up to 10 threads
$ Compressing objects: 100% (37/37), done.
$ Writing objects: 100% (44/44), 4.56 KiB | 2.28 MiB/s, done.
$ Total 44 (delta 4), reused 0 (delta 0), pack-reused 0
$ remote: Resolving deltas: 100% (4/4), done.
$ To github.com:YOCdot/gitLearn.git
$  * [new branch]      main -> main
$ branch 'main' set up to track 'origin/main'.
```

这样一来，`main`会跟踪`origin`的`main`。

查看分支情况：

```shell
git branch  # 查看本地分支
$   dl-feature
$ * main
git branch -a  # 查看所有分支，包括远程
$   dl-feature
$ * main
$   remotes/origin/main
git branch -r  # 查看远程分支
$   remotes/origin/main
```

虽然有远程分支，但是无法切换到这个远程分支，因为这个被`git`用来跟踪远程分支。

接下来推送`dl-feature`分支：

```shell
git push origin dl-feature  # 本次并未添加-u参数，所以本地的dl-feature分支并不会跟踪远程的同名分支的变化
$ Enumerating objects: 15, done.
$ Counting objects: 100% (15/15), done.
$ Delta compression using up to 10 threads
$ Compressing objects: 100% (9/9), done.
$ Writing objects: 100% (11/11), 1.16 KiB | 1.16 MiB/s, done.
$ Total 11 (delta 3), reused 0 (delta 0), pack-reused 0
$ remote: Resolving deltas: 100% (3/3), completed with 1 local object.
$ remote:
$ remote: Create a pull request for 'dl-feature' on GitHub by visiting:
$ remote:      https://github.com/YOCdot/gitLearn/pull/new/dl-feature
$ remote:
$ To github.com:YOCdot/gitLearn.git
$  * [new branch]      dl-feature -> dl-feature
```

## 27.`remote-workflow`：工作流

如果想要基于某个版本库进行开发，那么先`fork`一份，再在这份`fork`的仓库的基础上进行开发。

如果开发的新内容有用，可以添加到版本库中，可以申请`pull-request`来将新的提交添加到仓库中。

## 28.`clone`：克隆仓库

```shell
# git clone [URL] [文件夹名称]
git clone git@github.com:YOCdot/gitLearn.git new_gitLearn
```

然后可以对其进行修改和commit。

## 29.`fetch`：获取远程的更新

如果克隆了远程仓库之后，远程仓库又提交和PUSH了新内容，此时本地仓库可以使用：

```shell
git fetch
```

对新提交进行获取，如果新内容位于`main`分支，那么需要对其进行分支合并：

```shell
git merge origin/main
```

如此一来，本地仓库的内容就和`origin-main`一致了。

## 30.`fork`：保存一份当前状态的仓库到自己的远程中

在平台上`fork`一份之后，就可以使用`clone`命令克隆这一份仓库到本地。

如果要配置项目级别的name和e-mail：

```shell
git config user.name "xxx"
git config user.email "xxx@xxx.com"
```

## 31.`pull-request`：发起合并请求

在`fork`的仓库的基础之上如果进行了commit，如果觉得commit对原仓库有贡献，那么可以在平台上新建`pull-request`来对原仓库所有者发起拉取请求。

原所有者可以在自己的平台上查看详情，选择是否要合并。

## 32.`collaborator`：添加协作者

可以在项目设置中添加拥有写入权限的贡献者，所有操作均有效。

协作者如果没有配置全局`git`设置，要配置项目的`git`设置。

## 33.`github-tools`：官方图形工具

现在已经更名为<a href="https://desktop.github.com/">GitHub Desktop</a>

## 34.`brackets-git`：基于Brackets编辑器的插件

<a href="https://brackets.io/">官方网站</a>


