# MSR-test-repo
test 
1)Create two EC2 Instances in AWS Cloud using Terraform

--->First click on my security credintials in aws account
--->next click on create new access key
--->next click on show access_key and security key and copy both paste in terrfoam
provider aws
{
   access_key="AKIAI3K3VAA5EEKNJOBQ"
   secret_key="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"
   region="ap-southeast-2"
}
resource aws_instance"MSR-test-Instance-1"
{
ami="ami-0f9cf087c1f27d9b1"
instance_type="t2.micro"
key_name="123"
}
resource aws_instance"MSR-test-Instance-2"
{
ami="ami-0f9cf087c1f27d9b1"
instance_type="t2.micro"
key_name="123"
}
--->save file in .tf format
--->open git bash where terraform application located,
--->next enter terraform init 
--->Next Enter terraform Validate in git
--->Next Enter terraform plan in git
--->Next Enter terraform apply in git
--->Next automatically aws instance launch please check it once


*************************************************************************************************************************************************************

2)Software packages installation
A) We install the software packages using ansible playbook. Ansible playbook write inthe form of YAML.
---
- name: install softwear packages
  hosts: all
  tasks:
   - name: install docker
     shell: curl -fsSL https://get.docker.com -o get-docker.sh
   - name: install docker
     shell: sh get-docker.sh
   - name: install docker-compose
     shell: sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o                        /usr/local/bin/docker-compose
   - name: install openssl
     shell: https://www.openssl.org/source/openssl-1.1.1.tar.gz
            tar xvf openssl-1.1.1.tar.gz
   - name: install node 8.12.0
     shell: curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
            sudo apt-get install -y nodejs
   - name: install NVM – Version 0.33.2
     shell: curl https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash

   - name: install git
     shell: sudo apt-get install -y git

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
...

**********************************************************************************************************************************************************************

     
3A)A) vim docker compose .yml
---
 version: "3"
services:
  apache:
    container_name: "apachewebserver"
    image: "httpd"
    ports: 
     - 9090:80
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
