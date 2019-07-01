sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
# log out and log in to pickup the added group
# Also install some common sense stuff

# exit and ssh into instance again

docker ps
sudo chmod +x /usr/local/bin/docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname -s`-`uname -m` | sudo tee /usr/local/bin/docker-compose > /dev/null
sudo chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version


# getting materils
rm -r *
curl https://goblockchain.s3.amazonaws.com/2channels3members.zip --output content.zip
unzip content.zip

# running docker
export IMAGE_TAG=1.4
export ORDERER_HOST=54.163.133.203
export PEER0_ORG1_HOST=100.26.150.23
export PEER1_ORG1_HOST=100.26.150.23
export PEER0_ORG2_HOST=18.207.180.39
export PEER1_ORG2_HOST=18.207.180.39
export PEER0_ORG3_HOST=3.208.88.110
export PEER1_ORG3_HOST=3.208.88.110
docker-compose -f deployment/00_docker-compose-orderer.yaml up -d && docker ps

export IMAGE_TAG=latest
export ORDERER_HOST=54.163.133.203
export PEER0_ORG1_HOST=100.26.150.23
export PEER1_ORG1_HOST=100.26.150.23
export PEER0_ORG2_HOST=18.207.180.39
export PEER1_ORG2_HOST=18.207.180.39
export PEER0_ORG3_HOST=3.208.88.110
export PEER1_ORG3_HOST=3.208.88.110
docker-compose -f deployment/01_docker-compose-org1.yaml up -d && docker ps

export IMAGE_TAG=latest
export ORDERER_HOST=54.163.133.203
export PEER0_ORG1_HOST=100.26.150.23
export PEER1_ORG1_HOST=100.26.150.23
export PEER0_ORG2_HOST=18.207.180.39
export PEER1_ORG2_HOST=18.207.180.39
export PEER0_ORG3_HOST=3.208.88.110
export PEER1_ORG3_HOST=3.208.88.110
docker-compose -f deployment/02_docker-compose-org2.yaml up -d && docker ps

export IMAGE_TAG=latest
export ORDERER_HOST=54.163.133.203
export PEER0_ORG1_HOST=100.26.150.23
export PEER1_ORG1_HOST=100.26.150.23
export PEER0_ORG2_HOST=18.207.180.39
export PEER1_ORG2_HOST=18.207.180.39
export PEER0_ORG3_HOST=3.208.88.110
export PEER1_ORG3_HOST=3.208.88.110
docker-compose -f deployment/03_docker-compose-org3.yaml up -d && docker ps

# cli
export IMAGE_TAG=latest
export ORDERER_HOST=54.163.133.203
export PEER0_ORG1_HOST=100.26.150.23
export PEER1_ORG1_HOST=100.26.150.23
export PEER0_ORG2_HOST=18.207.180.39
export PEER1_ORG2_HOST=18.207.180.39
export PEER0_ORG3_HOST=3.208.88.110
export PEER1_ORG3_HOST=3.208.88.110
docker-compose -f deployment/04_docker-compose-cli.yaml up -d && docker ps


-----
# to clean up docker
docker-compose -f docker-compose-cli.yaml down
docker rm -f $(docker ps -aq)
docker rmi -f $(docker images | grep fabcar | awk '{print $3}')
docker volume prune
