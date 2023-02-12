# Installation de docker avec ansible 

## Packages utiles  
```shell
cd ~/tp-coaching-webforce3
sudo apt-get update  # update links to repos
sudo apt-get -y install git wget htop iotop iftop # install git and monitoring tools
sudo apt-get -y install python3 python3-venv # install python3 and virtualenv
sudo apt-get -y install build-essential   # need for installing docker-compose
sudo apt-get -y install python3-dev libxml2-dev libxslt-dev libffi-dev # need for installing docker-compose
htop  # check your vm config, cpu et memory
# Ctrl-C pour sortir de htop 
```
##  installation du repo docker et de docker
```shell
python3 -m venv venv  # set up the module venv in the directory venv
source venv/bin/activate  # activate the virtualenv python
pip3 install wheel  # set for permissions purpose
pip3 install --upgrade pip # update pip3
pip3 install ansible # install ansible 
pip3 install requests # extra packages
ansible --version # check the version number
cd install_docker
ansible-playbook -i inventory_for_ubuntu install_docker_ubuntu.yml --limit local  # run the playbook for installing docker
# close your IDE and start again and connect to your VM again  
cd ~/tp-coaching-webforce3  # 
source venv/bin/activate  # virtualenv
docker ps  # verification de docker
```
