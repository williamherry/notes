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

### Short version of `status`

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

#### generate commit hash

    # get the size of commit
    git cat-file commit HEAD | wc -c
    260

    # generate hash
    ( printf "commit 260\000"; git cat-file commit HEAD ) | sha1sum
    6bfa2893ae634c4284aa123a68378151ac11e347

    # compare
    git rev-parse HEAD
    6bfa2893ae634c4284aa123a68378151ac11e347

#### generate content hash

    # get the size of content
    git cat-file blob HEAD:welcome.txt | wc -c
    34

    # generate hash
    ( printf "blog 34\000"; git cat-file blob HEAD:welcome.txt ) | sha1sum
    74049a0123148161c26acb2812d290140665f5d3

    # compare
    git rev-parse HEAD:welcome.txt
    74049a0123148161c26acb2812d290140665f5d3

#### generate tree hash

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

    # list tags
    git tag

    # list tags and show one line of desc
    git tag -n1

    # wildcard
    git tag -l jx/v2*

    # use git log to show tags
    git log --oneline --decorate

    git describe

    # if work area has changes, can use --dirty to show up
    echo hacked >> README; git describe --dirty; dit checkout -- README

    git describe master^ --always

    git name-rev HEAD

    git name-rev HEAD --tags

    git log --pretty=oneline origin/helper/master | \
      git name-rev --tags --stdin

    # light tag
    git tag mytag

    # secret of light tag(just a point to one commit
    cat .git/refs/tags/mytag
    git cat-file -t mytag
    git cat-file -p mytag

Note: cons of light tag is that there is no way to know who and when create this tag, should not use this in team work

Note: git describe do not generate describe string use light tag

    # annotated tag
    git commit --allow-empty -m "blank commit for annotated tag test."
    git tag -m "My first annotated tag." mytag2
    git tag -l my* -n1

    # secret of annotated tag(create a tag object)
    cat .git/refs/tags/mytag2
    git cat-file -t mytag2
    git cat-file -p mytag2

    git cat-file tag mytag2 | wc -c
    (printf "tag 148\000"; git cat-file tag mytag2) | sha1sum

    # although tag mytag2 is a tag object, but most time can treat it as commit
    git log -1 --pretty=oneline mytag2

    # git rev-parse return the id of tag, not commit
    git rev-parse mytag2

    # way to get the commit id that tag point to
    git rev-parse mytag2^{commit}
    git rev-parse mytag2^{}
    git rev-parse mytag2^0
    git rev-parse mytag2~0

    # GPG-signed tag
    gpg --gen-key
    git tag -s -m "My first GPG-signed tag." mytag3

    # varify
    git tag -v mytag3

    # delete tag(delete tag can not be restore)
    git tag -d mytag

    # tag not push to remote when git push
    git push
    git ls-remote origin my*

    # specify tag when push
    git push origin mytag

    # push all tag to remote
    git push origin refs/tags/*

    # git pull will get tag
    git pull
    git tag -n1 -l my*

    # update tag
    git push origin mytag2
    git pull origin refs/tags/mytag2:refs/tags/mytag2

    # delete tag in remote
    git push <remote_url> :<tagname>
    git push origin :mytag2

    # name of tag can be check by check-ref-format
    git check-ref-format refs/tags/.name || echo "return $?, not valid ref"

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

### Interactive add file

    git add -i

### Gitignore

    # Show the file that has been ignored
    git status --ignored -s

    # ignored file can be added use add and -f option
    git add -f hello.h

    # local ignore
    .git/info/exclude
    # or
    git config --global core.excludesfile /home/william/.gitignore

    # example of .gitignore file
    *.a       # ignore all file that postfix with .a
    !lib.a    # do NOT ignore lib.a directory or file
    /TODO     # only ignore TODO file under /
    build/    # ignore all file under build/
    doc/*.txt # ignore all file like doc/notes.txt, but not doc/server/arch.txt

Note: .gitignore only work for files that untrached, file already add to index or repo are not affect

### Archive

    git archive -o latest.zip HEAD

    git archive -o partial.tat HEAD src doc

    git archive --format=tar --prefix=1.0 | gzip > foo-1.0.tar.gz

    git tar-commit-id latest.zip

### Count commits

    git rev-list HEAD | wc -l

### Git rev-parse command

    # list branch
    git rev-parse --symbolic --branches

    # list tags
    git rev-parse --symbolic --tags

    # show all refs
    git rev-parse --symbolic --glob=refs/*

    # translate Git object to hash
    git rev-parse HEAD

    git rev-parse master refs/heads/master

    git rev-parse 6652 6652a0d

    git rev-parse A refs/tags/A

    git rev-parse A^{} A^0 A^{commit}

    git rev-parse A^ A^1 B^0

    git rev-parse A^^3^2 F^2 J^{}

    git rev-parse A~3 A^^^ G^0

    git rev-parse A^{tree} A:

    git rev-parse A^{tree}:src/Makefile A:src/Makefile

    git rev-parse :gitg.png HEAD:gitg.png

    git rev-parse :/"Commit A"

    git rev-parse HEAD@{0} master@{0}

### Git log

    git log --oneline F^! D

    git --graph --oneline

    git git log -3 --pretty=oneline

    git log -p -1

    git log --stat --oneline I..C

    git log --pretty=raw -1

    git log --pretty=fuller -1

    git log --pretty=oneline -1

    git show D --stat

    git cat-file -p D^0

### Git diff

    git diff B A            # compate tag B and tag A
    git diff A              # diff work area and tag A
    git diff --cached A     # diff index area and tag A
    git diff                # diff work area and index area
    git diff --cached       # diff index area and HEAD
    git diff HEAD           # diff work area and HEAD

    git diff --word-diff

### Git blame

    git blame README

    git blame -L 6,+5 README

### Git bisect

    # manually
    git bisect start
    git bisect bad
    git bisect good G
    ls doc/B.txt
    git good
    ls doc/B.txt
    git bad
    ...

    # automatically
    cat > good-or-bad.sh <<-EOF
    #!/bin/sh

    [ -f doc/B.txt ] && exit 1
    exit 0
    EOF

    git bisect start master G
    git bisect run sh good-or-bad.sh

    git describe refs/bisect/bad

    # checkout example
    git ls-tree 776c5c9 README
    git ls-tree -r refs/tags/D doc

    git checkout HEAD^^

    git checkout refs/tags/D -- README
    git checkout 776c5c9 -- doc

    git show 776c5c9:README > README.OLD

    # reset bisect
    git bisect reset

### Git cherry-pick

    git cherry-pick E

### Misc

    git rev-list HEAD

    git ls-remote origin

### Gitolite

Error message:

    william@desktop:~/tmp$ git clone git@desktop:gitolite-admin
    Cloning into 'gitolite-admin'...
    fatal: 'gitolite-admin' does not appear to be a git repository
    fatal: The remote end hung up unexpectedly

Possible Reason:

    Note that you can also get this error if you have an ssh key in the remote servers authorized_keys file that isn’t prefixed with the “command” option. This can happen if you installed gitolite on a git user account that already had keys in that file. Simply delete or comment those keys out.

(Gitolite makes a copy of that auth file before it adds new keys, it must be a bug that it left it in)

### git clone error

Error message:

    Initialized empty Git repository in /root/notes/.git/

    (gnome-ssh-askpass:4841): Gtk-WARNING **: cannot open display:

Possible Resion(from git@irc.freenode.com):

    it seems like that you're using gnome-ssh-askpass to get authenticated which is a dialog, instand of commandline, and for some reason (a server or so), it cannot

    if SSH_ASKPASS set in env, try to unset it
