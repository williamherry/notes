### Great Alias

    # `commit -a` is not recommended by the author of Got Git
    #git config --global alias.ci "commit -a -v"

    git config --global alias.st "status"
    git config --global alias.co "checkout"
    git config --global alias.br "branch"

### Delete branch

    # delete locally
    git branch -D <branch_name>

    # delete in Github
    git push origin --delete <branch_name>

    # The above is syntactic sugar for
    git push origin :<branch_name>

### Fix last commit(`--amend`)

    git commit --amend
    git commit --amend --allow-empty --reset-author

### Enable color

    git config --global color.ui true

### Trace git status

    strace -e 'trace=file' git status

### Find `.git` location

    git rev-parse --git-dir

### Show the root directory of repo

    git rev-parse --show-toplevel

### The reletive path of current work directory to the root directory of repo

    git rev-parse --show-prefix

### The deep of current work directory to the root directory of repo

    git rev-parse --show-cdup

### Different options of `git config`

    git config -e
    git config -e --global
    git config -e --system

### Check the config

    git config core.editor

### Unset configure

    git config --unset --global user.name

### Make log look more nice

    git log --pretty=fuller

### Short version os `status`

    git status -s

### Use HEAD to reset index area, working area no affected

    git reset HEAD

### Remove file from index area, working area no affected

    git rm --cached <file>

### Use index area reset working area

    git checkout .
    git checkout -- <file>

### Reset index area and working area use master

    git checkout HEAD .
    git checkout HEAD <file>

### Check the tree of HEAD point to(`-l` means long format)

    git ls-tree -l HEAD

    # if you want see tree of index area, first you should write to Git object database
    git write-tree
    211a35625be8281584fd79be21d08a5d3a2ad350
    git ls-tree -l 211a

    # `-r` can recursely
    git write-tree | xargs git ls-tree -l -r -t

### Check the tree of index area

    git ls-files -s

### Git diff

    # diff work area with index area
    git diff

    # diff index area to HEAD
    git diff --cached
    # or
    git diff --staged

    # diff work area with HEAD
    git diff HEAD

### Save the work that not ready for commit(I'll be back)

    git status
