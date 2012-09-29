### Shortcut for commit(from happercasts)

    git config --global alias.ci "commit -a -v"

### Delete branch

    # delete locally
    git branch -D <branch_name>

    # delete in Github
    git push origin --delete <branch_name>

    # The above is syntactic sugar for
    git push origin :<branch_name>
