# fabric explorer

Fabric-explorer is a simple, powerful, easy-to-use, highly maintainable, open source fabric browser. Fabric-explorer can reduce the difficulty of learning and using fabric, so that we can intuitively feel the fabric of the powerful features.

## Directory Structure
```
├── app                    fabric GRPC interface
├── artifacts              
├── blockdata              the fabric data struct sample
├── db			   the mysql script and help class
├── explorer_client        Web Ui
├── listener               websocket listener
├── metrics                metrics about tx count per minute and block count per minute
├── service                the service 
├── socket		   push real time data to front end
├── timer                    
└── utils                    
```


## Requirements

* docker 1.12.6
* docker-compose 1.11.2
* golang 1.8
* nodejs 6.9.5
* git
* mysql

## Database setup
Run the database setup scripts located under `db/fabricexplorer.sql`

`mysql -u<username> -p < db/fabricexplorer.sql`

## start fabric-explorer

1. `cd /opt/gopath/src/github.com/hyperledger/fabric/examples/e2e_cli/`
2. `git clone https://github.com/shipx123/fabric-explorer.git`
3. `rm -rf ./artifacts/crypto-config/`
4. `cp -r $GOPATH/src/github.com/hyperledger/fabric/examples/e2e_cli/crypto-config ./artifacts`

5. modify config.json,set channel,mysql,tls (if you use tls communication, please set  enableTls  true ,if not set false) 
```json
 "channelsList": ["mychannel"],
 "enableTls":true, 
 "mysql":{
      "host":"172.16.10.162",
      "database":"fabricexplorer",
      "username":"root",
      "passwd":"123456"
   }
```

6. modify app/network-config.json or app/network-config-tls.json(if you use tls communication) 

```json
 {
	"network-config": {
		"orderer": [{
			"url": "grpc://192.168.6.139:7050",
			"server-hostname": "orderer.example.com"
		}],
		"org1": {
			"name": "peerOrg1",
			"mspid": "Org1MSP",
			"ca": "http://192.168.6.139:7054",
			"peer1": {
				"requests": "grpc://192.168.6.139:7051",
				"events": "grpc://192.168.6.139:7053",
				"server-hostname": "peer0.org1.example.com"
			},
			"peer2": {
				"requests": "grpc://192.168.6.139:8051",
				"events": "grpc://192.168.6.139:8053",
				"server-hostname": "peer1.org1.example.com"
			},
			"admin": {
				"key": "/artifacts/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore",
				"cert": "/artifacts/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/signcerts"
			}
		},
		"org2": {
			"name": "peerOrg2",
			"mspid": "Org2MSP",
			"ca": "http://192.168.6.139:8054",
			"peer1": {
				"requests": "grpc://192.168.6.139:9051",
				"events": "grpc://192.168.6.139:9053",
				"server-hostname": "peer0.org2.example.com"
			},
			"peer2": {
				"requests": "grpc://192.168.6.139:10051",
				"events": "grpc://192.168.6.139:10053",
				"server-hostname": "peer1.org2.example.com"
			},
			"admin": {
				"key": "/artifacts/crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp/keystore",
				"cert": "/artifacts/crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp/signcerts"
			}
		}
	}
}

```
8. `./start.sh`
