# Airport-Licensing

Airport Licensing Bussiness Network

Built on Hyperledger Composer.
```
airportlicensing

org.example.airportlicensing
```

## Deploying a network

```
cd ~/fabric-dev-servers

./startFabric.sh

nvm use 8

composer-playground
```

package.json network version

```
nvm use 8

cd ~/WebProjects/airport-licensing

composer archive create -t dir -n .

composer network install -a airport-licensing@<v>.bna -c PeerAdmin@hlfv1

composer network start --networkName airport-licensing --networkVersion <v> --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card

composer card import --file networkadmin.card

composer network ping --card admin@airport-licensing

```

## Upgrading a Deployed Network

```
cd ~/fabric-dev-servers

export FABRIC_VERSION=hlfv12

./startFabric.sh

nvm use 8

composer-playground
```

package.json network version

```
nvm use 8

cd ~/WebProjects/airport-licensing

composer archive create -t dir -n .

composer network install -a airport-licensing@<v>.bna -c PeerAdmin@hlfv1

composer network upgrade -c PeerAdmin@hlfv1 -n airport-licensing -v <v>

composer card import --file networkadmin.card

composer network ping --card restadmin@airport-licensing

```

## Deploying rest-server

```
docker run -d --name mongo --network composer_default -p 27017:27017 mongo

cd rest-server && docker build -t myorg/composer-rest-server .

cd ..

source envvars.txt

composer card import --file networkadmin.card

composer network ping --card admin@airport-licensing

composer participant add -c admin@airport-licensing -d '{"$class":"org.hyperledger.composer.system.NetworkAdmin", "participantId":"restadmin"}'

composer identity issue -c admin@airport-licensing -f restadmin.card -u restadmin -a "resource:org.hyperledger.composer.system.NetworkAdmin#restadmin"

composer card import -f  restadmin.card

composer network ping -c restadmin@airport-licensing

sed -e 's/localhost:7051/peer0.org1.example.com:7051/' -e 's/localhost:7053/peer0.org1.example.com:7053/' -e 's/localhost:7054/ca.org1.example.com:7054/' -e 's/localhost:7050/orderer.example.com:7050/' < $HOME/.composer/cards/admin@airport-licensing/connection.json  > /tmp/connection.json && cp -p /tmp/connection.json $HOME/.composer/cards/admin@airport-licensing/

cd rest-server && docker-compose up

cd ..

composer card export -c admin@airport-licensing -f restadmin-plus-cert.card
```

## Exporting cards

```
composer identity issue -c admin@airport-licensing -f <name>.card -u email -a "resource:org.example.airportlicensing.<Participant>#<email>"

composer card import --file networkadmin.card

composer card export -c admin@airport-licensing -f <name>-plus-cert.card
```

## Stoping Server

```
cd ~/fabric-dev-servers

./stopFabric.sh

```

## Docker Stop

```
docker kill $(docker ps -q)
docker rm $(docker ps -aq)
docker rmi $(docker images dev-* -q)
```
### License
```
MIT
```
