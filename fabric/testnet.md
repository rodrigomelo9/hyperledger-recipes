# Hyperledger Fabric test network

> Based on [Using the Fabric test network](https://hyperledger-fabric.readthedocs.io/en/latest/test_network.html).

## Preparation

You must always work into the `test-network` directory: `cd fabric-samples/test-network`

## Bring up the test network

```bash
./network.sh up [createChannel [-c CHANNEL_NAME]] [-s couchdb] [-ca]
```

Where:
* `createChannel [-c channelName]`: create *mychannel* or *channelName* when specified.
* `-s couchdb`: specify to use couchDB instead of levelDB.
* `-ca`: used to add Certificate Authorities.

You can use `docker ps` to check the quantity of nodes:
* Default is three: One orderer and two peers.
* Two are added if couchDB is used.
* Three are added if `-ca-` is specified.

Channel creation can be also performed when the network is running with `./network.sh createChannel [-c channelName]`

You can start a chaincode on the channel using `./network.sh deployCC [-c channelName]`

> **Note:** two more nodes are added, one by peer, to run the chaincode (`docker ps` to verify it).

# Interacting with the network

```bash
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
```

Environment variables for Org1:
```bash
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
```

Initialize the ledger with assets:
```bash
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"InitLedger","Args":[]}'
```

Get the list of assets that were added to your channel ledger:
```bash
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'
```

Change the owner of an asset on the ledger:
```bash
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'
```

Environment variables for Org2
```bash
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051
```

Query the asset-transfer:
```bash
peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'
```

> **Note:** in the previous commands, change *mychannel* for *channelName* if specified.

# Bring down the network

```bash
./network.sh down
```

> **Note:** it must be run between `./network.sh up` invocations.
