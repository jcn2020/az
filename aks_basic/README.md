## Create cluster 

### setup - likely done by install script 
- AZ_REPO=$(lsb_release -cs)
- echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |      sudo tee /etc/apt/sources.list.d/azure-cli.list
- sudo apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893
- sudo apt-get install apt-transport-https
- sudo apt-get update && sudo apt-get install azure-cli

- az
- az aks

### Create resourceGroup from EastUS
- az login
- az aks create -g jneastus -n mycluster --generate-ssh-keys
- sudo az aks install-cli

### step may take some time 
- sudo az aks get-credentials -g jneastus -n mycluster

- kubectl deploy -f deploy.yaml

- kubetcl get nodes


