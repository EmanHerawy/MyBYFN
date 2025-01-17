# MyBYFN

This repo is just for learning how to create Fabric network (BYFN) manually . This network consists of :
1- Solo ordering service 
2- Two Orgs :
        SawTooth with 2 Peers and tow users
        Fabric with 3 peers and two users
#Done
1-Crypto Generation
2-Create a Channel Configuration Transaction
3- Start the network
4- Create & Join Channel
# To do
1-Install & Instantiate Chaincode
2- invoke the chaincode
3- Node SDK 

## Want to Run It ?! run the following commands, you can find them in my_steps.tx file


#1-Crypto Generator 
 #creat my crypro config files , copy cryptogen tool to create crypro matrials 
echo "========== Crypto Generator  =========="

bin/cryptogen generate --config=./crypto-config.yaml
bin/configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
# if it throws error create the channel-artifact folder and run it again
 

# 2-Create a Channel Configuration Transaction
echo "========== Create a Channel Configuration Transaction =========="
# plz note AVOID any _ -  use just letters I tried to use them and it throws error when I tried to create the chnnel
export CHANNEL_NAME=hlchannel
echo "==========  channel: "$CHANNEL_NAME" =========="

./bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME
 
#define ancher peers 
#org 1
echo "==========  ancher peers =========="

./bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/SawtoothMSPanchors.tx -channelID $CHANNEL_NAME -asOrg SawtoothMSP
 
#// for Fabric
echo "========== Sawtooth Org ancher peers  =========="

./bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/FabricMSPanchors.tx -channelID $CHANNEL_NAME -asOrg FabricMSP
echo "========== Fabric Org ancher peers =========="

#3- Start the network
echo "========== Start the network =========="

docker-compose -f docker-compose-cli.yaml up -d

# 4- Create & Join Channel
docker start cli


# Environment variables for PEER0


echo "========== Create & Join Channel: "$CHANNEL_NAME" =========="

docker exec -it cli bash

export ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/HLStudyGroup.com/orderers/orderer.HLStudyGroup.com/msp/tlscacerts/tlsca.HLStudyGroup.com-cert.pem
export CHANNEL_NAME=hlchannel

# Channel creation
peer channel create -o orderer.HLStudyGroup.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls $CORE_PEER_TLS_ENABLED --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/HLStudyGroup.com/orderers/orderer.HLStudyGroup.com/msp/tlscacerts/tlsca.HLStudyGroup.com-cert.pem
echo "========== peer channel create -o orderer channel: "$CHANNEL_NAME" =========="



export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Sawtooth.HLStudyGroup.com/users/Admin@Sawtooth.HLStudyGroup.com/msp
export CORE_PEER_ADDRESS=peer0.Sawtooth.HLStudyGroup.com:7051
export CORE_PEER_LOCALMSPID="SawtoothMSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Sawtooth.HLStudyGroup.com/peers/peer0.Sawtooth.HLStudyGroup.com/tls/ca.crt


# peer0 join the channel 
echo "========== peer0 join the channel: "$CHANNEL_NAME" =========="

 peer channel join -b $CHANNEL_NAME.block

export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Sawtooth.HLStudyGroup.com/users/Admin@Sawtooth.HLStudyGroup.com/msp
export CORE_PEER_ADDRESS=peer1.Sawtooth.HLStudyGroup.com:7051
export CORE_PEER_LOCALMSPID="SawtoothMSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Sawtooth.HLStudyGroup.com/peers/peer1.Sawtooth.HLStudyGroup.com/tls/ca.crt


# peer1 join the channel 
echo "========== peer1 join the channel: "$CHANNEL_NAME" =========="

 peer channel join -b $CHANNEL_NAME.block


export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/users/Admin@Fabric.HLStudyGroup.com/msp
export CORE_PEER_ADDRESS=peer0.Fabric.HLStudyGroup.com:7051
export CORE_PEER_LOCALMSPID="FabricMSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/peers/peer0.Fabric.HLStudyGroup.com/tls/ca.crt


# peer0 join the channel 
echo "========== peer0 join the channel: "$CHANNEL_NAME" =========="

 peer channel join -b $CHANNEL_NAME.block

export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/users/Admin@Fabric.HLStudyGroup.com/msp
export CORE_PEER_ADDRESS=peer1.Fabric.HLStudyGroup.com:7051
export CORE_PEER_LOCALMSPID="FabricMSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/peers/peer1.Fabric.HLStudyGroup.com/tls/ca.crt


# peer1 join the channel 
echo "========== peer1 join the channel: "$CHANNEL_NAME" =========="

 peer channel join -b $CHANNEL_NAME.block

export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/users/Admin@Fabric.HLStudyGroup.com/msp
export CORE_PEER_ADDRESS=peer2.Fabric.HLStudyGroup.com:7051
export CORE_PEER_LOCALMSPID="FabricMSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/peers/peer2.Fabric.HLStudyGroup.com/tls/ca.crt


# peer2 join the channel 
echo "==========  peer2 join the channel: "$CHANNEL_NAME" =========="

 peer channel join -b $CHANNEL_NAME.block


echo "========== Start Update the anchor peers in channel: "$CHANNEL_NAME" =========="

#Update the anchor peers
#for sawtooth
peer channel update -o orderer.HLStudyGroup.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/SawtoothMSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/HLStudyGroup.com/orderers/orderer.HLStudyGroup.com/msp/tlscacerts/tlsca.HLStudyGroup.com-cert.pem

#for fabric
#  
#CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/users/Admin@Fabric.HLStudyGroup.com/msp CORE_PEER_ADDRESS=peer0.Fabric.HLStudyGroup.com:7051 CORE_PEER_LOCALMSPID="FabricMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/Fabric.HLStudyGroup.com/peers/peer0.Fabric.HLStudyGroup.com/tls/ca.crt

 peer channel update -o orderer.HLStudyGroup.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/FabricMSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/HLStudyGroup.com/orderers/orderer.HLStudyGroup.com/msp/tlscacerts/tlsca.HLStudyGroup.com-cert.pem

echo "========== end  Update the anchor peers in channel : "$CHANNEL_NAME" =========="