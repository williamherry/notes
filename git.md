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

### Git cat-file

    git cat-file -t e698609
    git cat-file -p 3698609

    for id in 3698609 e698609; do \
    ls .git/objects/${id:0:2}/${id:2}*; done

### Trace the commit use git log

    git log --pretty=raw --graph e69089

### Secrets of HEAD and master

    git log -1 HEAD
    git log -1 master
    git log -1 refs/heads/master

    find .git -name HEAD -o -name master

    cat .git/HEAD

    git rev-parse master
    git rev-parse refs/heads/master
    git rev-parse HEAD

### generate the hash(`\000` means <null>)

- generate commit hash

    # get the size of commit
    git cat-file commit HEAD | wc -c
    260

    # generate hash
    ( printf "commit 260\000"; git cat-file commit HEAD ) | sha1sum
    6bfa2893ae634c4284aa123a68378151ac11e347

    # compare
    git rev-parse HEAD
    6bfa2893ae634c4284aa123a68378151ac11e347

- generate content hash

    # get the size of content
    git cat-file blob HEAD:welcome.txt | wc -c
    34

    # generate hash
    ( printf "blog 34\000"; git cat-file blob HEAD:welcome.txt ) | sha1sum
    74049a0123148161c26acb2812d290140665f5d3

    # compare
    git rev-parse HEAD:welcome.txt
    74049a0123148161c26acb2812d290140665f5d3

- generate tree hash

    # get the size of tree
    git cat-file tree HEAD^{tree} | wc -c
    39

    # generate hash
    ( printf "tree 39\000"; git cat-file tree HEAD^{tree} ) | sha1sum
    d55bcfbe6787e8b5b1a6071d2f26890951491291

    # compare
    git rev-parse HEAD^{tree}
    d55bcfbe6787e8b5b1a6071d2f26890951491291

### Reset

    git reset --hard HEAD^

    git reset --hard 9e8a761

    tailf -5 .git/logs/refs/heads/master

    git reflog show master | head -5

    git reset --hard master@{2}

### Checkout

    # checkout <branch>, update HEAD point to <branch>, update work area and index area from branch
    git checkout <branch>

    # report diff of work area, index area and HEAD
    git checkout

    # update <filename> in work area from index area, no mercy
    git checkout -- <filename>

    # with HEAD not change, update <filename> of work area and index area from <filename> of <branch>
    git checkout <branch> -- <filename>

    # update all file in work area from index area, no mercy
    git checkout -- .
    # or
    git checkout .

### Stash(not very clean)

    # Save the work that not ready for commit(I'll be back)
    git stash

    # list
    git stash list

    # pop most recent stash
    git stash pop

    # save stash
    git stash save "message..."

    # drop stash
    git stash drop [<stash>]

    # clear all stash
    git stash clear

    # create branch based on stash
    git stash branch <branchname> <stach>

    # apply stash
    git stash apply [--index] [<stash>]

### Clean

    # dry-run
    git clean -nd

    # force
    git clean -fd

### Tag

    git tag -m "Say bye-bye to all previous practice." old_practice
    git describe

### Delete and restore file

    # delete files(using: git rm)
    rm -rf *.txt
    git rm detached-commit.txt hack-1.txt new-commit.txt welcome.txt
    git commit -m "delete trash files. (using: git rm)"

    # delete files(using: git add -u)
    rm -rf *.txt
    git add -u
    git commit -m "delete trash files. (using: git add -u)"

    # restore file(using: cat-file)
    git cat-file -p HEAD~1:welcome.txt > welcome.txt

    # restore file(using: show)
    git show HEAD~1:welcome.txt > welcome.txt

    # restore file(using: checkout)
    git checkout HEAD~1 -- welcome.txt

### Move

    git mv welcome.txt README
