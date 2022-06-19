# netology-ansible
В hosts прописаны две машины:

[all]
node1 ansible_host=192.168.107.132 ansible_user=ansible ansible_password=ansible
node2 ansible_host=192.168.107.135 ansible_user=ansible ansible_password=ansible

В конфигурационном файле:
[defaults]

inventory      = /etc/ansible/hosts
host_key_checking = False
log_path = /var/log/ansible.log
no_log = False

YAML-файл:

- name: Home Works
  hosts: all
  become: yes
 
  tasks:
  - name: Task ping
    ping:
 
  - name: Task yum update
    yum:
      update_cache: yes
 
  - name: Install net-tools, git, tree, htop, mc, vim
    yum:
      name:
        - net-tools
        - git
        - tree
        - htop      
        - mc
        - vim  
      state: latest
  - name: Copy File
    copy:
      src: test.txt
      dest: /home/ansible/files
      mode: 0777
  - name: Create User
    user: 
      name: "{{item.user_name}}"
      shell: /bin/bash/
      append: yes
      home: "/home/{{item.home_dir}}"
    with_items:
      - {user_name: devops_1, home_dir: home_devops_1}
      - {user_name: test_1, home_dir: test_1}
