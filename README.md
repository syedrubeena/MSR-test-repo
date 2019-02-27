# MSR-test-repo
test 
1) Create two EC2 instance
A) To launch  an EC2 instance below resources are required
1.AMI -----> ubuntu 16.04LTS
2.Select instance type-----> t2.micro
3.Configure instance details----->here we give the information about instancees. here we take number of instances are 2
                                  and we check Auto-assign public ip Enables.
4.Add storages------> here we add the storage of volumes .t2.micro is our instance type so it allocate the 8GiB of storage to 
                      to our instance.
5.Add tags-----> here we mention the names of that two instances
                 1.MSR-test-instance-1
                 2.MSR-test-instance-2
6.Configure security group------> SG is a virtual firewall. it can attached to the EC2instances.
                                  a) SG configuration is of two parts i.e Inbound & Outbound i.e SSH,all traffic,TCP/UDP etc these are some SG
What type of requests server allows will be configured in INbound rules
What type of requests server can send ,will be configured in Outbound  rules.

7. Review and launch .scroll down to review the details of AMI,instance type,Security group etc
8. At the key pair we select the choosen existing or new key pair
check the check box to acknowledge that you have access to private key
9. Click launch instances

*************************************************************************************************************************************************************

2)Software packages installation
A) We install the software packages using ansible playbook. Ansible playbook write inthe form of YAML.
---
- name: Install docker ,docker compose,NVM,Nodejs
  hosts: all
  tasks:
   - name: install docker
     shell: curl-FssL http://get.docker o get-docker.sh
   - name: install docker
     shell: sh get-docker.sh
   - name: install docker compose
     shell: sudo curl -L "https://github.com/docker/compose/releases/download/1.13/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   - name: install docker compose
     shell: sudo chmod +x /usr/local/bin/docker-compose
   - name: install NVM
     shell: curl https://raw.githubusercontent.com/creationix/nvm/v0.7.0/install.sh | sh
    creates=/home/{{ ansible_user_id }}/.nvm/nvm.sh
   - name: Install node and set version
     shell: >/bin/bash -c "source ~/.nvm/nvm.sh && nvm install 0.33.2 && nvm alias default 0.10"creates=/home/{{ ansible_user_id }}/.nvm/alias
   - name: install openssl
     shell: wget -c https://www.openssl.org/source/openssl-1.0.2p.tar.gz
            $ tar -xzvf openssl-1.0.2p.tar.gz
    - name: Install git
      shell: sudo apt-get install -y git
...
By using this play book we install all the related softwares
NOTE: To run the playbook we use this command
ansible-playbook playbookname.yml

**********************************************************************************************************************************************************************

4)docker compose file for couch DB
A) vim docker compose .yml
---
 version: "3"
services:
  couchdb:
    container_name: "aph-couchdb"
    image: "aph/couchdb"
    networks:
      - backend
    volumes:
      - cb-combined:/opt/couchdb/data
networks:
  backend:
  frontend:
...

**********************************************************************************************************************************************************************

5) Git commit Step by step procedure
A) The "commit" command is used to save your changes to the local repository.
Git which changes you want to include in a commit before running the "git commit" command.
This means that a file won't be automatically included in the next commit just because it was changed. 
Instead, you need to use the "git add" command to mark the desired changes for inclusion.
Also in Git, a commit is not automatically transferred to the remote server. 
Using the "git commit" command only saves a new commit object in the local Git repository. 
Exchanging commits has to be performed manually and explicitly (with the "git fetch", "git pull", and "git push" commands).
 
 syntax of git commit ----> git commit -m "commitname"

example: first you create some files f1,f2,f3
touch f1 f2 f3
2. to send the all files &folders from working directory to stragging
     git add.
3.To send the file from stragging area to local repository we use git commit command
    git commit -m "some name"
4. To see the commit history
    git log
5. To enter git push command all files moves local repository to remote repostory
6. Goto  repository edir readme file
 

*************************************************************************************************************************************************
     
3A)A) vim docker compose .yml
---
 version: "3"
services:
  apache:
    container_name: "apachewebserver"
    image: "apache:2"
    ports: 
     - 9090:80
    networks:
      - backend
    volumes:
      - cb-combined:/opt/couchdb/data
...
--->Next save the docker compose file.
--->create playbook for automate the entaire installation of apache and deploy into git
sudo vim playbook1.yml
---
- name: build the docke compose file
  hosts: all
  tasks:
   - name: docker compose up
     shell: sudo docker-compose.yml up
   - name: download the html file from git link
     git:
      repo: enter ur git hub repository link
      dest: /var/www/html/index.html
    - name: wait for 3 seconds
      pause:
       seconds: 3
    - name: to restart the service
      service:
      name: apche2
      state: restart
   - name: check url response
     uri:
      url: http:// ip address of nodes
      status: 200
...
if respons is 200 it passes the playbook other than 200 its false
NOTE: if you run the ansible playbook 
ansible-playbook playbookname.yml


***********************************************************       THANK YOU     ***********************************************************************************
