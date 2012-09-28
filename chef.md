## install chef server on redhat/centos

    rpm -Uvh http://rbel.frameos.org/rbel6
    yum -y install rubygem-chef-server
    service iptables stop
    setup-chef-server.sh
    knife configure -i

## install chef client on redhat/centos

   curl -L http://www.opscode.com/chef/install.sh | bash


