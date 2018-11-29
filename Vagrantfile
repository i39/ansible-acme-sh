Vagrant.configure("2") do |config|

    # main & default: normal OS series...
    config.vm.define "main", primary: true do |node|
        node.vm.box = "ubuntu/xenial64"
        #node.vm.box = "ubuntu/bionic64"
        #node.vm.box = "debian/jessie64"
        #node.vm.box = "debian/stretch64"

        node.vm.provision "ansible" do |ansible|
            ansible.playbook = "test.yml"
            ansible.sudo = true
        end
    end


    # alpine: special case...
    config.vm.define "alpine" do |node|
        node.vm.box = "maier/alpine-3.8-x86_64"

        # for Alpine Linux...
        node.vm.synced_folder '.', '/vagrant', disabled: true

        node.vm.provision "ansible" do |ansible|
            ansible.playbook = "test-alpine.yml"
            ansible.sudo = true
            ansible.extra_vars = { alpine_python_fix_repo: true }
        end
    end


    # docker: for auto build & testing (e.g., Travis CI)
    config.vm.define "docker" do |node|
        node.vm.box = "williamyeh/ubuntu-trusty64-docker"

        node.vm.provision "shell", inline: <<-SHELL
            cd /vagrant
            docker build  -f test/Dockerfile-ubuntu18.04  -t nginx_xenial   .
            docker build  -f test/Dockerfile-ubuntu16.04  -t nginx_bionic  .
            docker build  -f test/Dockerfile-debian8      -t nginx_jessie   .
            docker build  -f test/Dockerfile-debian7      -t nginx_wheezy   .
            docker build  -f test/Dockerfile-alpine3      -t nginx_alpine3  .
        SHELL
    end

end
