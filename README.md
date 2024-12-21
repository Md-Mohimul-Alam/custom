

export PATH=${PWD}/bin:$PATH

cryptogen generate --config=./crypto-config.yaml --output=./crypto-config  configtxgen -profile OrdererGenesis -channelID system-channel -outputBlock ./channel-artifacts/orderer.genesis.block

docker-compose -f docker-compose.yaml down 
docker-compose -f docker-compose.yaml up -d


 channel1 block::

 configtxgen -profile Channel1 -outputCreateChannelTx ./channel-artifacts/channel1.tx -channelID channel1 

export CORE_PEER_TLS_ENABLED=false  
export CORE_PEER_LOCALMSPID="Hospital1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/crypto-config/peerOrganizations/hospital1.ehr.com/peers/peer0.hospital1.ehr.com/tls/ca.crt 
export CORE_PEER_MSPCONFIGPATH=${PWD}/crypto-config/peerOrganizations/hospital1.ehr.com/users/Admin@hospital1.ehr.com/msp
export CORE_PEER_ADDRESS=localhost:7051



peer channel create -o orderer.ehr.com:7050 -c channel1 -f ./channel-artifacts/channel1.tx --outputBlock ./channel-artifacts/channel1.block

  
peer channel join -b ./channel-artifacts/channel1.block


peer channel list

peer channel fetch config ./channel-artifacts/config_block.pb -o orderer.ehr.com:7050 -c channel1


 channel2 block::

export CORE_PEER_TLS_ENABLED=false 
export CORE_PEER_LOCALMSPID="Hospital2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/crypto-config/peerOrganizations/hospital2.ehr.com/peers/peer0.hospital2.ehr.com/tls/ca.crt 
export CORE_PEER_MSPCONFIGPATH=${PWD}/crypto-config/peerOrganizations/hospital2.ehr.com/users/Admin@hospital2.ehr.com/msp
export CORE_PEER_ADDRESS=peer0.hospital2.ehr.com:8051


   

configtxgen -profile Channel2 -outputCreateChannelTx ./channel-artifacts/channel2.tx -channelID channel2                    


peer channel create -o orderer.ehr.com:7050 -c channel2-f ./channel-artifacts/channel2.tx --outputBlock ./channel-artifacts/channel2.block

  

peer channel join -b ./channel-artifacts/channel2.block


peer channel list

peer channel fetch config ./channel-artifacts/config_block.pb -o orderer.ehr.com:7050 -c channel2






___ if you want to create a channel using Hospital1
____

export CORE_PEER_LOCALMSPID="Hospital1MSP"
export CORE_PEER_ADDRESS=peer0.hospital1.ehr.com:7051
export CORE_PEER_TLS_ENABLED=false

____ f you want to use the Research Institute peer
_____
export CORE_PEER_LOCALMSPID="ResearchInstituteMSP"
export CORE_PEER_ADDRESS=peer0.research.ehr.com:9051
export CORE_PEER_TLS_ENABLED=false


_____
And for Hospital 2:
————

export CORE_PEER_LOCALMSPID="Hospital2MSP"
export CORE_PEER_ADDRESS=peer0.hospital2.ehr.com:8051
export CORE_PEER_TLS_ENABLED=false
