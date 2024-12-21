


export PATH=${PWD}/fabric-samples/bin:$PATH
export FABRIC_CFG_PATH=${PWD}/fabric 

cryptogen generate --config=./crypto-config.yaml --output=./crypto-config  configtxgen -profile OrdererGenesis -channelID system-channel -outputBlock ./channel-artifacts/orderer.genesis.block


docker-compose -f docker-compose.yaml up -d

configtxgen -profile Channel1 -outputCreateChannelTx ./channel-artifacts/channel1.tx -channelID channel1    

configtxgen -profile Channel2 -outputCreateChannelTx ./channel-artifacts/channel2.tx -channelID channel2                    


peer channel create -o orderer.ehr.com:7050 -c channel1 -f ./channel-artifacts/channel1.tx --outputBlock ./channel-artifacts/channel1.block

peer channel create -o orderer.ehr.com:7050 -c channel2-f ./channel-artifacts/channel2.tx --outputBlock ./channel-artifacts/channel2.block

 export CORE_PEER_MSPCONFIGPATH=/Users/mohimthesaesk/Documents/customFabric/crypto-config/peerOrganizations/hospital1.ehr.com/users/Admin@hospital1.ehr.com/msp
 
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Hospital1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/hospital1.ehr.com/peers/peer0.hospital1.ehr.com/tls/ca.crt 
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/hospital1.ehr.com/users/Admin@hospital1.ehr.com/msp
export CORE_PEER_ADDRESS=localhost:7051
