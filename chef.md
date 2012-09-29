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

# The chef-client runs in two stages

1. The compilation phase
   The client examines each *Recipe* in order and adds its *Resources* to the *ResourceCollection*

2. The execution phase
   The client iterates over the ResourceCollection, and performs the following:

- Based on the Resource's "provider" attribute, a new instance of the specified Provider is created (if the attribute is not set, one is selected based on the local platform).
  The originating Resource is stored in the new Provider as the `@new_resource` instance variable and is accessible when writing LWPs as `new_resource.name`, for example.

- The `load_current_resource` method is then called on the provider instance, giving it an opportunity to populate `@current_resource` with a new resource that represents the current state of the relevant part of the system.

- For each action specified in `@new_resource.actions`, the `action_` method that corresponds to each action is called(e.g. `action :create will` invoke the `action_create` method of the Provider instance.)

