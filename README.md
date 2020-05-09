# ansible
Learning Ansible: examples of playbooks & snippets
==================
General tips
------------------
1. To install Ansible on your master machine use the appropriate package management tool. Since Debian was used by me, it is as follows:
~~~bash
    sudo apt update && sudo apt install ansible -y
~~~
To be sure ansible is installed and works properly, just check its version as follows:
~~~bash
    root@your-host:~/ansible# ansible --version
    ansible 2.7.7
      config file = /root/ansible/ansible.cfg
      configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
      ansible python module location = /usr/lib/python3/dist-packages/ansible
      executable location = /usr/bin/ansible
      python version = 3.7.3 (default, Dec 20 2019, 18:57:59) [GCC 8.3.0]
    root@your-host:~/ansible#
~~~
2. First we need to set up ssh key for connection (I decided to name this one `id_rsa_default`): 
 ~~~bash
     ssh-keygen -t -f /root/.ssh/id_rsa_default.pub rsa -C "your.email@email.com"
~~~ 
3. Then we add the public key to our `targets` servers using Ansible (to do this we should add `ansible_ssh_pass` to our `inventory` file first):
~~~yaml
      tasks:
      # Adding key
        - name: Set authorized key taken from file
          authorized_key:
            user: root
            state: present
            key: "{{ lookup('file', '/root/.ssh/id_rsa_default.pub') }}"`
  ~~~
4. After adding the key to `targets` we can remove `ansible_ssh_pass` from `inventory` file fo it is safe to share.
5. To check syntax of `playbook` we could use the following command:
  ~~~yaml
      ansible-playbook snippets.yaml --syntax-check
  ~~~
Result should be as following:
  ~~~yaml
   root@your-host:~/ansible# ansible-playbook snippets.yaml --syntax-check

   playbook: snippets.yaml
  ~~~
6. If syntax is OK we could do the dry run to check the actual operation of our playbook:
~~~bash
    ansible-playbook snippets.yaml --check
~~~
7. Finally if we see no errors in the output of out previous command, we can proceed to real execution of the playbook:
~~~bash
    ansible-playbook snippets.yaml 
~~~
