# asset-transfer-secured-agreement
Note: Lab #1 must be completed first

## Part 1 - setup and executing the private transaction

```
sudo reboot ## restart your vm to free up resources
sudo apt install jq
sudo apt install golang-go
cd fabric-samples/asset-transfer-secured-agreement/chaincode-go
GO111MODULE=on go mod vendor
```
Go into the test directory
```
cd ../../test-network
sudo ./network.sh down  
sudo ./network.sh up createChannel -ca -c mychannel -s couchdb
sudo chmod a+rwx -R organizations  ## this is only done for lab env
```
Set env varaibles and deploy chaincode
```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
sudo ./network.sh deployCC -ccn secured -ccp ../asset-transfer-secured-agreement/chaincode-go/ -ccl go -ccep "OR('Org1MSP.peer','Org2MSP.peer')"
```

Set env variables for Org 1
```
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_ADDRESS=localhost:7051
```

Use another terminal window, set the env variables for Org 2
```
export PATH=${PWD}/../bin:${PWD}:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_ADDRESS=localhost:9051
```

Follow the remainder of the instructions starting at "Create an asset" in the Hyperledger Fabric tutorial https://hyperledger-fabric.readthedocs.io/en/latest/secured_asset_transfer/secured_private_asset_transfer_tutorial.html 


## Part 2 - Viewing the private and public data in CouchDB

We can port forward from our local machine to the virutal machine and then access the CouchDB UI using port 5984.

ssh usage:
```
ssh -L local_port:destination_server_ip:remote_port ssh_server_hostname
```
An example on how to connect is
```
ssh -i lab2.pem -L 5984:ec2-54-91-100-220.compute-1.amazonaws.com:5984 ubuntu@ec2-54-91-100-220.compute-1.amazonaws.com
```
Now, we can access the CouchDB UI locally by using the browser and go to:
```
http://localhost:5984/_utils/#login
username: admin
password: adminpw
```
Go to mychannel_secured and inspect the data for peer0 in Org1

Do the same for Org2, you will need to create a new tunnel and use the port 6984 to see the Couch DB for the peer0 on Org2.

