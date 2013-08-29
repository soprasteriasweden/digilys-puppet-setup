# Puppet-based server setup for Digilys

This repo contains the basis for setting up a server for running Digilys.

## Caveats

This setup uses Puppet to configure the server, but not in a conventional way.
Some reconfiguration of Puppet is required (see Installation instructions
below), and it is not advisable to use this in a regular Puppet setup without
modification.

## Installation instructions

The installation instructions are for CentOS 6, which is the recommended
operating system for running Digilys. All commands are executed on the server as
root. Use `sudo` if necessary.

1. Install git:

    ~~~
    $ yum install git
    ~~~

2. Install Puppet repository, Puppet and required Puppet modules:

    ~~~
    $ rpm -ivh http://yum.puppetlabs.com/el/6/products/i386/puppetlabs-release-6-7.noarch.rpm
    $ yum install puppet
    $ puppet module install puppetlabs/postgresql
    $ puppet module install alup/rbenv
    $ puppet module install thias/nginx
    ~~~

3. Install PostgreSQL repository:

    ~~~
    $ rpm -ivh http://yum.postgresql.org/9.2/redhat/rhel-6-i386/pgdg-centos92-9.2-6.noarch.rpm
    ~~~

4. Install nginx repository:

    ~~~
    $ rpm -ivh http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm
    ~~~

5. Clone this repo, placing it in `/opt/digilys` (required):

    ~~~
    $ git clone https://github.com/kentorgbg/digilys-puppet-setup.git /opt/digilys
    ~~~

6. Add the following lines to `/etc/puppet/puppet.conf`, under the `[main]`
   section:

    ~~~
    # Use Digilys Puppet modules
    modulepath = $confdir/modules:/usr/share/puppet/modules:/opt/digilys/modules

    # Use Digilys hiera config
    hiera_config = /opt/digilys/etc/hiera.yaml
    ~~~

7. Add a Hiera config for your specific server in
   `/opt/digilys/etc/hiera/digilys-custom.yaml`. See
   `/opt/digilys/etc/hiera/digilys-common.yaml` for defaults and options.

8. Run the setup by executing:

    ~~~
    $ puppet apply /opt/digilys/digilys.pp
    ~~~

