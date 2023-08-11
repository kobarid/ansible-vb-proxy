# **ansible-vb-proxy**

Simple ansible playbook to deploy Virtualbox servers and install [Portainer](https://www.portainer.io/), [nginx](https://nginx.org/) and [haproxy](http://www.haproxy.org/) as some basic proxy.

## Prerequisites

* an ansible controller node with `ansible` >= **2.13.8**
  * on [Ubuntu](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu)

## Deployment
1. Git clone the repository
```bash
git clone https://github.com/kobarid/ansible-vb-proxy.git
```
2. Deploy VirtualBox and Vagrant on your host
```bash
ansible-playbook vbox.yaml
```
3. Create ssh key (leave blank the pass)
```bash
ssh-keygen -t rsa
```
4. Modify the `Vagrantfile`` if needed with your allowed ssh user
```ruby
ssh_pub_key = File.readlines("/home/ubuntu/.ssh/id_rsa.pub").first.strip
``` 
5. Start the VMs
```bash
vagrant up
```
6. When you first run the playbook `prov.yaml`, you should first ssh to each VMs to add keys to known_hosts
```bash
ssh vagrant@192.168.56.101
ssh vagrant@192.168.56.102
ssh vagrant@192.168.56.103
```  
7. Provision the VMs with docker engine, Portainer, nginx and haproxy
```bash
ansible-playbook -i hosts prov.yaml
```
8. Check if the containers are alive
```bash
curl http://192.168.56.102:8080 ; echo
curl -k https://192.168.56.103 ; echo
curl http://192.168.56.103 ; echo
```
They should all respond with **I'm alive**

## Limitations //TODOS
* The deployment was tested on a Ubuntu 20.04.03 LTS Linux Ansible controller host
  - [ ] Test on other Distributions
* The VMs are configured with a private network
  - [ ] Figure out network connectivity

## References
* https://www.virtualbox.org/manual/ch06.html
* https://dmarko.tcl-digitrade.com/post/2021/ansible-docker-with-portainer/
* https://jhooq.com/vagrant-copy-public-key/
* https://stackoverflow.com/a/67392439/14809031
* https://gist.github.com/shirou/6928012
