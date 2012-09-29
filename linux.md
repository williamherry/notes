### Use parted to partion a disk(in script)

    # install parted of course
    yum -y install parted

    # initialize disk(have to run this and only need run once on a disk)
    parted /dev/vdc --script -- mklabel msdos

    # part all size as a swap partion
    parted /dev/vdc --script -- mkpartfs primary linux-swap 0 -1

    # mkswap(format)
    mkswap /dev/vdc1

    # initialize(`vdb` this time)
    parted /dev/vdb --script -- mklabel msdos

    # first partion 30G, second partion all left
    parted /dev/vdb --script -- mkpart primary 0 30G
    parted /dev/vdb --script -- mkpart primary 30G 100%

    # mkfs(format)
    mkfs.ext3 /dev/vdb1
    mkfs.ext4 /dev/vdb2
