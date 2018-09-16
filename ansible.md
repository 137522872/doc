

[TOC]

## Ansible



### centos7 安装ansible

```shell
1、 python版本需要2.6以上，不过通过centos7都会默认安装上python2.7.5，查看方法：python -V
2、 添加yum 源
　　　　a、 vim /etc/yum.repos.d/ansible
　　　　b、 添加如下内容：
　　　　　　[epel]
　　　　　　name = all source for ansible
　　　　　　baseurl = https://mirrors.aliyun.com/epel/7/x86_64/
　　　　　　enabled = 1
　　　　　　gpgcheck = 0
　　　　　　
　　　　　　[ansible]
　　　　　　name = all source for ansible
　　　　　　baseurl = http://mirrors.aliyun.com/centos/7.3.1611/os/x86_64/
　　　　　　enabled = 1
　　　　　　gpgcheck = 0
　　　　　　
　　3、 yum clean all
　　4、 安装ansible：yum install ansible -y
```

### ansible核心组件

> ansible core
>
> host iventory
>
> core modules
>
> custom modules
>
> playbook 
>
> connect plugin
>
> 

asnible 基于python实现,由paramiko ,plymal 和jinjia2 模块

------

```shell
rpm -ql ansible

/etc/ansible/ansible.cfg
/etc/ansible/hosts
```



```shell
man ansible-doc
#列出所有模块
ansible-doc -l
#查看模块如何使用
ansible-doc -s MODULE_NAME

#执行命令
ansible 192.168.2.109 -m command -a "ls /usr/local"
ansible web -m command -a "date"


```

- [ ] command 命令模块 ,默认模块,用于在远程执行命令 ansible all -a 'date'

  ```she
  cron:
  	state:
  		present: 安装
  		absent: 移除
  #添加一个定时任务
  ansible demo -m cron -a " minute='*/10' job='/bin/echo hello'  name='test cron job' state=present "
  
  #移除定时任务
  ansible demo -m cron -a " minute='*/10' job='/bin/echo hello'  name='test cron job' state=absent " 
  ansible demo -a "crontab -l"
  
  ```

  

  #### user模块

  ```shell
  user模块
  #添加用户
  ansible all -m user -a " name='user1' "
  #移除用户
  ansible all -m user -a " name='user1' state=absent "
  
  ```

  

  #### group模块

  ```shell
  group模块
  
  #添加组
  ansible demo -m group -a " name='mysql' gid=3306 system='yes' "
  
  ```

  

  #### copy模块

  ```shell
  copy模块
  src=本地源文件路径
  ansible 192.168.2.101 -m copy -a "src='./Python-3.5.1.tgz' dest='/home/os' "
  
  # 用content指定文件内容
  ansible 192.168.2.101 -m copy -a "content='hello world' dest='/home/os/demo.txt' "
  
  ```

  

  #### file模块

```shell
file模块
ansible all -m file -a "owner=python group=python  mode=644 path=/home/os/Python-3.5.1.tgz"
```

#### ping 模块

```shell
#ping模块
ansible all -m ping
```

#### service模块

```shell
service:控制服务的运行状态
ansible-doc -s service
ansible demo -a 'systemctl list-dependencies sshd.service'
#开启服务
ansible demo -m service -a "enabled=true name=sshd state=started"
```

#### script模块

```shell
#将本地脚本复制到远程主机并运行
ansible demo -m script -a "test.sh"
```

#### yum模块

```shell
#安装vim
ansible demo -m yum -a "name=vim"
#卸载vim
ansible demo -m yum -a "name=vim state=absent"
```

#### setup模块

```shell
#收集远程主机信息
ansible demo -m setup
```



### yml

```shell
- hosts: demo
  tasks:
  - name: create nginx group
    group: name=nginx system=yes
    sudo: yes
  - name:
    user: name=nginx system=yes
    sudo: yes

```

```shell
- hosts: demo
  tasks:
  - name: install httpd package
    yum: name=httpd state=latest
  - name: install configuration file for httpd
    copy: src=/home/os/httpd.conf dest=/etc/httpd/conf/httpd.conf
  - name: start httpd service
    service: enabled=true name=httpd state=started

```

```shell
- hosts: demo
  tasks:
  - name : copy file
    copy: content="{{ansible_all_ipv4_addresses}}" dest=/tmp/vars.ansible

使用变量
```

```shell
- hosts: demo
  vars:
  - username: user10
  - num : 2
  tasks:
  - name: create {{username}} user
    user: name= {{username}}
    when: num==2
条件语句
```

```shell

```

