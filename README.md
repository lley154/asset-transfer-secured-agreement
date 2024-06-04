# asset-transfer-secured-agreement
Note: Lab #1 must be completed first

## Setup and executing the private transaction

```
sudo reboot ## restart your vm to free up resources
```
```
sudo apt install jq
```
```
sudo apt install golang-go
```
```
cd fabric-samples/asset-transfer-secured-agreement/chaincode-go
```
```
GO111MODULE=on go mod vendor
```
Go into the test directory
```
cd ../../test-network
```
```
./network.sh down  
```
```
./network.sh up createChannel -ca -c mychannel
```
```
sudo chmod a+rwx -R organizations  ## this is only done for lab env
```
```
sudo chmod a+rwx log.txt
```
Set env varaibles and deploy chaincode
```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
```
```
sudo ./network.sh deployCC -ccn secured -ccp ../asset-transfer-secured-agreement/chaincode-go/ -ccl go -ccep "OR('Org1MSP.peer','Org2MSP.peer')"
```

Set env variables for Org 1
```
cd fabric-samples/test-network
```
```
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_ADDRESS=localhost:7051
```

Use another terminal window, set the env variables for Org 2
```
cd fabric-samples/test-network
```
```
export PATH=${PWD}/../bin:${PWD}:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_ADDRESS=localhost:9051
```

Follow the remainder of the instructions starting at "Create an asset" in the Hyperledger Fabric tutorial [secured_private_asset_transfer_tutorial.html](https://hyperledger-fabric.readthedocs.io/en/latest/secured_asset_transfer/secured_private_asset_transfer_tutorial.html#create-an-asset)

