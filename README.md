# asset-transfer-basic-go

Note: Lab 1 must be already completed
```
sudo reboot ## restart your vm to free up resources
sudo apt install golang-go
cd fabric-samples/asset-transfer-basic/chaincode-go
GO111MODULE=on go mod vendor
```
Go into the test network directory
```
cd ../../test-network
./network.sh down  
sudo ./network.sh up createChannel -ca -c mychannel
sudo chmod a+rwx log.txt ## this is only done for lab env
sudo chmod a+rwx -R organizations  ## this is only done for lab env
```
Export paths and config directory
```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
```
The chain code should be successfully deployed.
Export the following envs to invoke the contract
```
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
```
Invoke the chaincode
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"InitLedger","Args":[]}'
```
Query the chaincode
```
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'
```



