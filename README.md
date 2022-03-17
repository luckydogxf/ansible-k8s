# ansible-k8s
deploy k8s master and worker nodes automatically.

# how to run it ?
1. create your own `site.yml` like:
```
---
- name: testing...
  hosts: k8s
  gather_facts: yes
  ignore_errors: yes
  remote_user: ubuntu
  become: yes
  roles:
    - k8s-setup
    - .... <Other roles>
  environment:
    http_proxy: http://172.16.232.21:8088
    https_proxy: http://172.16.232.21:8088
    no_proxy: no_proxy=10.96.0.0/12,172.16.232.234,172.16.232.21,172.16.215.0/24,172.16.215.81,.xyz.com,10.244.0.0/16,127.0.0.1
    
```
2. Build your own inventory accordingly.
```
[all:vars]
# comment out if you use SSH Keypair.
ansible_ssh_pass='XXXXXX'

[pacemaker]
172.16.215.41
172.16.215.63
172.16.215.155

[k8s]
172.16.215.79
172.16.215.77
```
3. run it
```
ansible-playbook -i hosts site.yml -b -u ubuntu
```
4. Note
User `ubuntu` must exist in target hosts and has `sudo` privileges.
