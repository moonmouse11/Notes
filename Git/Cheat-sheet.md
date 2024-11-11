# Cheat-sheet
***
## Settings
``` bash
#---------------------------------------------------------------------
# НАСТРОЙКИ
#---------------------------------------------------------------------

# Показать глобальную конфигурацию:
git config --global --list

git config --local user.name "user name"
git config --local user.email "user@email.com"
git config --local core.fileMode false
git config --local core.quotepath false
git config --local core.autocrlf true

# GPG-подпись коммитов:
git config --local commit.gpgsign true
git config --local user.signingkey 8E4426F38FBFE1F9
# Ключ можно указать любым методом, который поддерживает gpg.

# Установка текстового редактора:
git config --global core.editor "atom --wait"
git config --global core.editor "code --wait"
git config --global core.editor "/Applications/Sublime\ Text.app/Contents/MacOS/Sublime\ Text -n -w"
git config --global core.editor "mate -w"
```
***
## Changes
``` bash
#---------------------------------------------------------------------
# ИЗМЕНЕНИЯ
#---------------------------------------------------------------------

# Сделать unstage всех файлов (противоположность git add --all):
git reset

# Удалить staged-изменения:
git reset HEAD
# git-reset - Reset current HEAD to the specified state

# HEAD is a reference to the last commit in the currently check-out branch.
# You can think of the HEAD as the "current branch". 
# When you switch branches with git checkout, the HEAD revision changes to point to the tip of the new branch.
# You can see what HEAD points to by doing: cat .git/HEAD
# It is possible for HEAD to refer to a specific revision that is not associated with a branch name. 
# This situation is called a detached HEAD.


# Удалить все untracked-изменения:
git clean -df
# git-clean - Remove untracked files from the working tree
# -d          Remove untracked directories in addition to untracked files. 
#             If an untracked directory is managed by a different Git repository, it is not removed by default. 
#             Use -f option twice if you really want to remove such a directory.
# -f --force  If the Git configuration variable clean.requireForce is not set to false, git clean will refuse to delete files or directories unless given -f, -n or -i. 
#             Git will refuse to delete directories with .git sub directory or file unless a second -f is given.

# Удалить все unstaged-изменения:
git checkout -- .

# Удалить все untracked-изменения и все unstaged-изменения (отменить все текущие изменения):
git clean -df && git checkout -- .


# Посмотреть историю изменений файла (с учетом того, что он мог быть переименован):
git log --follow -- /path/to/file.txt
# --follow  Continue listing the history of a file beyond renames (works only for a single file).

# Когда diff показывает изменения он показывает вокруг изменения три строки (контекст).
# Можно показывать только изменения:
git diff --unified=0 q9tvkp8ak9n0d53c3ihq6u0gso9qd759m4vw09wa~ q9tvkp8ak9n0d53c3ihq6u0gso9qd759m4vw09wa
# -U<n> --unified=<n>  Generate diffs with <n> lines of context instead of the usual three. 
#                      Implies --patch. Implies -p.

# Сравнить изменения файла между двумя коммитами (ревизиями) через внешний инструмент:
git difftool ahlht78aeiygfmbe1zm164j7z5ykops4wjsgwqw1:path/to/file.ext 226d6n0autuc9tfp1lcrbcj2ortr4ykwqwf3qlja:path/to/file.ext
# git-difftool - Show changes using common diff tools

# Показать файл из ревизии:
git show REVISION:/path/to/file

# Создать архив из измененных файлов:
tar czf changed-files.tar.gz `git ls-files --modified`
zip modified-files.zip $(git ls-files --modified)
```
***
## Commits
``` bash
#---------------------------------------------------------------------
# КОММИТЫ
#---------------------------------------------------------------------

# Пустой коммит:
git commit -m "root" --allow-empty

# Удалить последний комммит, а изменения оставить в index, они будут в staged и их можно сразу закомитить:
git reset --soft HEAD^

# Отменить коммит и закоммитить изменения:
git revert <commit>
# git-revert - Revert some existing commits

# Отменить коммит, но не коммитить изменения:
git revert -n <commit>
# -n --no-commit  Usually the command automatically creates some commits with commit log messages stating which commits were reverted. 
#                 This flag applies the changes necessary to revert the named commits to your working tree and the index, but does not make the commits. 
#                 In addition, when this option is used, your index does not have to match the HEAD commit. 
#                 The revert is done against the beginning state of your index.
#                 This is useful when reverting more than one commits' effect to your index in a row.

# Удалить последний коммит:
git reset --hard HEAD^
# --hard  Resets the index and working tree. 
#         Any changes to tracked files in the working tree since <commit> are discarded.

# Удалить последние два коммита:
git reset --hard HEAD~2


# ref~ is shorthand for ref~1 and means the commit's first parent. 
# ref~2 means the commit's first parent's first parent. 
# ref~3 means the commit's first parent's first parent's first parent. 
# And so on.

# ref^ is shorthand for ref^1 and means the commit's first parent. 
# But where the two differ is that ref^2 means the commit's second parent (remember, commits can have two parents when they are a merge).

# The ^ and ~ operators can be combined.

# https://stackoverflow.com/a/43046393/9370853


# Обновить дату последнего коммита:
GIT_COMMITTER_DATE="$(date)" git commit --amend --no-edit --date "$(date)"

# Как применить коммит, но не коммитить:
git cherry-pick -n <commit>

# Показать ветки в которых есть коммит:
git branch --contains <commit>

# Показать ветки (в том числе удаленные) в которых есть коммит:
git branch -r --contains <commit>

# Привести файл к виду из заданной ревизии:
git checkout <commit> -- /path/to/file.txt

# Запушить заданный коммит:
git push origin <commit>:refs/heads/master
```
***
## Branches
``` bash
#---------------------------------------------------------------------
# ВЕТКИ
#---------------------------------------------------------------------

# Показать локальные и удаленные ветки:
git branch -a
# git-branch - List, create, or delete branches
# Option -r causes the remote-tracking branches to be listed, and option -a shows both local and remote branches.
# -a --all     List both remote-tracking branches and local branches.

# Показать удаленные ветки:
git branch -r
# -r --remotes  List or delete (if used with -d) the remote-tracking branches.

# Создать новую ветку и перейти в неё:
git checkout -b new_f
# If you are creating a branch that you want to checkout immediately, it is easier to use the git checkout command with its -b option to create a branch and check it out with a single command.

# Создать локальную ветку из коммита и перейти в неё:
git checkout -b feature/something 55sfu31q81q5epea8u0cdx7nzdeivpl6kjslsjhq

# Сделать так чтобы локальная ветка feature/something отслеживала удаленную ветку origin/feature/something:
git branch --set-upstream-to=origin/feature/something feature/something
# Можно сделать позже при push-е в remote:
git push --set-upstream origin feature/something
# Локальная ветка будет отслеживать удаленную, но в данном случае мы не делаем этого раньше времени.

# Отменить привязку к удаленному источнику:
git branch --unset-upstream

# Создать локальную ветку my_branch отслеживающую удаленную origin/my_branch и переключиться на неё:
git checkout --track origin/my_branch
# --track is shorthand for
git checkout -b [branch] [remotename]/[branch]

# Удалить локальную ветку (только если она за-push-ена и c-merge-на с удаленной отслеживаемой веткой):
git branch -d branch_name
# -d --delete  Delete a branch. 
#              The branch must be fully merged in its upstream branch, or in HEAD if no upstream was set with --track or --set-upstream-to.

# Удалить локальную ветку независимо от ее push или merge статуса:
git branch -D branch_name
# -D          Shortcut for --delete --force.
# -f --force  Reset <branchname> to <startpoint>, even if <branchname> exists already. 
#             Without -f, git branch refuses to change an existing branch. 
#             In combination with -d (or --delete), allow deleting the branch irrespective of its merged status. 
#             In combination with -m (or --move), allow renaming the branch even if the new branch name already exists, the same applies for -c (or --copy).

# Удалить удаленную ветку:
git push origin --delete feature/something

# Переименовать текущую локальную ветку:
git branch -m new-name
```
***
## Tags
``` bash
#---------------------------------------------------------------------
# ТЕГИ
#---------------------------------------------------------------------

# Показать локальные теги:
git tag

# Показать теги на remote:
git ls-remote --tags origin
```
***
## Repository
``` bash
#---------------------------------------------------------------------
# РЕПОЗИТОРИИ
#---------------------------------------------------------------------

# GitHub:
#  HTTPS: https://github.com/YOUR_USER/YOUR_REPOSITORY.git
#  SSH: git@github.com:YOUR_USER/YOUR_REPOSITORY.git
#  GitHub CLI: gh repo clone YOUR_USER/YOUR_REPOSITORY
git remote add com.github.gusenov.YOUR_REPOSITORY com.github.gusenov:gusenov/YOUR_REPOSITORY.git
git remote add com.github.gusenov.YOUR_REPOSITORY.wiki com.github.gusenov:gusenov/YOUR_REPOSITORY.wiki.git
$ cat ~/.ssh/config
Host com.github.gusenov
 HostName github.com
 User git

# GitLab:
git remote add com.gitlab.gusenov.YOUR_REPOSITORY com.gitlab.gusenov:gusenov/YOUR_REPOSITORY.git
git remote add com.gitlab.gusenov.YOUR_REPOSITORY.wiki com.gitlab.gusenov:gusenov/YOUR_REPOSITORY.wiki.git
$ cat ~/.ssh/config
Host com.gitlab.gusenov
 HostName gitlab.com
 User git

# Bitbucket:
#  HTTPS: https://YOUR_USER@bitbucket.org/gusenov/YOUR_REPOSITORY.git
#  SSH: git@bitbucket.org:YOUR_USER/YOUR_REPOSITORY.git
git remote add org.bitbucket.gusenov.YOUR_REPOSITORY org.bitbucket.gusenov:gusenov/YOUR_REPOSITORY.git
git remote add org.bitbucket.gusenov.YOUR_REPOSITORY.wiki org.bitbucket.gusenov:gusenov/YOUR_REPOSITORY.git/wiki
$ cat ~/.ssh/config
Host org.bitbucket.gusenov
 HostName bitbucket.org
 User git

# Helix TeamHub:
#  HTTP: https://YOUR_USER@helixteamhub.cloud/YOUR_COMPANY/projects/YOUR_PROJECT/repositories/git/YOUR_REPOSITORY
#  SSH: hth@helixteamhub.cloud:YOUR_COMPANY/projects/YOUR_PROJECT/repositories/git/YOUR_REPOSITORY
$ git remote add cloud.helixteamhub.gusenov.YOUR_REPOSITORY cloud.helixteamhub.gusenov:gusenov/projects/YOUR_PROJECT/repositories/git/YOUR_REPOSITORY
$ git remote add cloud.helixteamhub.gusenov.YOUR_PROJECT.wiki cloud.helixteamhub.gusenov:gusenov/projects/YOUR_PROJECT/repositories/git/wiki
$ cat ~/.ssh/config
Host cloud.helixteamhub.gusenov
 HostName helixteamhub.cloud
 User hth

# JetBrains Space:
#  SSH: ssh://git@git.jetbrains.space/gusenov/YOUR_PROJECT/YOUR_REPOSITORY.git
#  HTTPS: https://git.jetbrains.space/gusenov/YOUR_PROJECT/YOUR_REPOSITORY.git
git remote add space.jetbrains.gusenov.YOUR_REPOSITORY space.jetbrains.gusenov:gusenov/YOUR_PROJECT/YOUR_REPOSITORY.git
$ cat ~/.ssh/config
Host space.jetbrains.gusenov
 HostName git.jetbrains.space
 User git
 IdentityFile ~/.ssh/gusenov.jetbrains.space-id_rsa
```
***
## Repository Defense
``` bash
#---------------------------------------------------------------------
# ЗАЩИТА РЕПОЗИТОРИЕВ
#---------------------------------------------------------------------

# git-remote-gcrypt
git remote add cryptremote gcrypt::git@bitbucket.org:user/gcrypt-sample.git
git config remote.cryptremote.gcrypt-participants <GPG key fingerprint>
git config remote.cryptremote.gcrypt-signingkey <GPG key fingerprint>
```
***
