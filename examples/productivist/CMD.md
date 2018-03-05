# Command Line Interface for Productivist

## Installation
### Clone Productivist
```
git clone -b Productivist https://github.com/Productivist/fabric.git
cd fabric
``` 

### Prerequisites
```
curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0-alpha
```
or 
```
make
```
add your bin folder to your PATH
```
export PATH=$PWD/bin:$PATH
```

## Productivist
### Go to 
```
cd fabric/examples/productivist
```

### Start the network
```
./network_setup.sh up
```

### Inside the docker cli
```
docker exec -it cli bash
```

### set up the channel name
```
export CHANNEL_NAME=productivist
```

### Check orderer
```
CORE_PEER_LOCALMSPID="OrdererMSP"
CORE_PEER_TLS_ROOTCERT_FILE=$ORDERER_CA
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/productivist.com/orderers/orderer.productivist.com/msp

peer channel fetch 0 0_block.pb -o orderer.productivist.com:7050 -c "testchainid" --tls --cafile $ORDERER_CA
```

### Create channel
```
CORE_PEER_LOCALMSPID="PrivateMSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/private.productivist.com/peers/peer0.private.productivist.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/private.productivist.com/users/Admin@private.productivist.com/msp

peer channel create -o orderer.productivist.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/$CHANNEL_NAME.tx --tls --cafile $ORDERER_CA
```

### Join peer0.private
```
peer channel join -b $CHANNEL_NAME.block
```

### Join peer1.private
```
env | grep CORE | grep peer0 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer0/peer1/g")
env | grep CORE | grep peer1 

peer channel join -b $CHANNEL_NAME.block
```

### Join peer0.public
```
env | grep CORE | grep peer1 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer1/peer0/g")
env | grep CORE | grep peer0 


env | grep CORE | grep private 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/private/public/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/private/public/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/private/public/g")
export CORE_PEER_LOCALMSPID=PublicMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/private/public/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/private/public/g")
env | grep CORE | grep -i public 

peer channel join -b $CHANNEL_NAME.block
```

### Join peer1.public
```
env | grep CORE | grep peer0 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer0/peer1/g")
env | grep CORE | grep peer1 

peer channel join -b $CHANNEL_NAME.block
```

### Update peer0.private
```
env | grep CORE | grep peer1 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer1/peer0/g")
env | grep CORE | grep peer0 

env | grep CORE | grep public
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/public/private/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/public/private/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/public/private/g")
export CORE_PEER_LOCALMSPID=PrivateMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/public/private/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/public/private/g")
env | grep CORE | grep -i private 

peer channel update -o orderer.productivist.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/${CORE_PEER_LOCALMSPID}anchors.tx --tls --cafile $ORDERER_CA
```

### Update peer0.public
```
env | grep CORE | grep private 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/private/public/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/private/public/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/private/public/g")
export CORE_PEER_LOCALMSPID=PublicMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/private/public/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/private/public/g")
env | grep CORE | grep -i public 

peer channel update -o orderer.productivist.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/${CORE_PEER_LOCALMSPID}anchors.tx --tls --cafile $ORDERER_CA
```

### Install chaincode on peer0.private
```
env | grep CORE | grep public
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/public/private/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/public/private/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/public/private/g")
export CORE_PEER_LOCALMSPID=PrivateMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/public/private/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/public/private/g")
env | grep CORE | grep -i private 

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02
```

### Install chaincode on peer1.private
```
env | grep CORE | grep peer0 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer0/peer1/g")
env | grep CORE | grep peer1 

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02
```

### Install chaincode on peer0.public
```
env | grep CORE | grep peer1 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer1/peer0/g")
env | grep CORE | grep peer0 

env | grep CORE | grep private 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/private/public/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/private/public/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/private/public/g")
export CORE_PEER_LOCALMSPID=PublicMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/private/public/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/private/public/g")
env | grep CORE | grep -i public 

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02
```

### Install chaincode on peer1.public
```
env | grep CORE | grep peer0 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer0/peer1/g")
env | grep CORE | grep peer1 

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02
```

### Instantiate chaincode on peer0.private
```
env | grep CORE | grep peer1 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer1/peer0/g")
env | grep CORE | grep peer0 

env | grep CORE | grep public
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/public/private/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/public/private/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/public/private/g")
export CORE_PEER_LOCALMSPID=PrivateMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/public/private/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/public/private/g")
env | grep CORE | grep -i private 

peer chaincode instantiate -o orderer.productivist.com:7050 --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P "OR  ('PrivateMSP.member','PublicMSP.member')"
```

### Query, Invoke, Query on peer0.private
```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

peer chaincode invoke -o orderer.productivist.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

### peer1.private
```
env | grep CORE | grep peer0 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer0/peer1/g")
env | grep CORE | grep peer1 

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

peer chaincode invoke -o orderer.productivist.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

### peer0.public
```
env | grep CORE | grep peer1 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer1/peer0/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer1/peer0/g")
env | grep CORE | grep peer0 

env | grep CORE | grep private 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/private/public/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/private/public/g")
export CORE_PEER_MSPCONFIGPATH=$(echo $CORE_PEER_MSPCONFIGPATH | sed -e "s/private/public/g")
export CORE_PEER_LOCALMSPID=PublicMSP
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/private/public/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/private/public/g")
env | grep CORE | grep -i public 

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

peer chaincode invoke -o orderer.productivist.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

### peer1.public
```
env | grep CORE | grep peer0 
export CORE_PEER_ADDRESS=$(echo $CORE_PEER_ADDRESS | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_ROOTCERT_FILE=$(echo $CORE_PEER_TLS_ROOTCERT_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_KEY_FILE=$(echo $CORE_PEER_TLS_KEY_FILE | sed -e "s/peer0/peer1/g")
export CORE_PEER_TLS_CERT_FILE=$(echo $CORE_PEER_TLS_CERT_FILE | sed -e "s/peer0/peer1/g")
env | grep CORE | grep peer1 

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

peer chaincode invoke -o orderer.productivist.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

## execute end2end scenario
```
./scripts/script.sh
```

## Check orderer newest
```
CORE_PEER_LOCALMSPID="OrdererMSP"
CORE_PEER_TLS_ROOTCERT_FILE=$ORDERER_CA
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/productivist.com/orderers/orderer.productivist.com/msp

peer channel fetch newest -o orderer.productivist.com:7050 -c "testchainid" --tls --cafile $ORDERER_CA
```

## Stop an and clean everything

### Check what's up
```
watch 'docker ps -a && docker images'
```

### Stop the network
```
./network_setup.sh down
```

### clean the remaining docker network
```
docker network prune
```

## Use couchDB
```
./network_setup.sh up prductivist 10000 couchdb
```