# Install chef server on redhat/centos

    rpm -Uvh http://rbel.frameos.org/rbel6
    yum -y install rubygem-chef-server
    service iptables stop
    setup-chef-server.sh
    knife configure -i

# Install chef client on redhat/centos

    curl -L http://www.opscode.com/chef/install.sh | bash

# Resources and Providers

    In Chef, Resources represent a piece of system state and Providers are the underlying implementation which brings them into that state.
