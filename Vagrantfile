# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'getoptlong'

VAGRANTFILE_API_VERSION = '2'

# Default box type.  To launch with another use:
#
# vagrant --box=<box type> up
# e.g. vagrant --box=centos7 up
#
active_box='ubuntu1404'

opts = GetoptLong.new(
  [ '--box', GetoptLong::OPTIONAL_ARGUMENT ]
)

opts.each do |opt, arg|
  case opt
  when '--box'
    active_box=arg
  end
end

boxen = {
  'centos6' => {
    name: 'puppetlabs/centos-6.6-64-nocm',
    sfolder: '/home/vagrant/sync',
  },
  'centos7' => {
    name: 'centos/7',
    sfolder: '/home/vagrant/sync',
  },
  'ubuntu1404' => {
    name: 'ubuntu/trusty64',
    sfolder: '/vagrant',
  },
  'wheezy64' => {
    name: 'debian/wheezy64',
    sfolder: '/vagrant',
  },
  'jessie64' => {
    name: 'debian/jessie64',
    sfolder: '/vagrant',
  },
}
fail 'box is not defined' unless boxen.has_key?(active_box)

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = boxen[active_box][:name]
  config.vm.synced_folder ".", boxen[active_box][:sfolder], disabled: true

  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.provision 'ansible' do |ansible|
    ansible.sudo     = true
    ansible.verbose  = true
    ansible.playbook = 'tests/pre.yml'
  end
  config.vm.provision 'ansible' do |ansible|
    ansible.sudo     = true
    ansible.verbose  = true
    ansible.playbook = 'tests/main.yml'
  end
  config.vm.provision 'ansible' do |ansible|
    ansible.sudo     = true
    ansible.verbose  = true
    ansible.playbook = 'tests/post.yml'
  end
end
