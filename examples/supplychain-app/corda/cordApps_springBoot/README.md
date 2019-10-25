# Supply Chain Corda

 This project consists of the cordapp, cordapp-supply-chain and cordapp-contract-states . Nodes with the cordapp-supply-chain may interact with the two TrackableStates; ProductState and a ContainerState as defined in cordapp-contract-states.

## But first, what is the Supply Chain Application?

 To be able to understand this application's usage you may visit (if you have access): https://alm.accenture.com/wiki/display/BLOCKOFZ/Supply+Chain+App

 But if you don't have access to above link, below is the high-level description of the application instead:

 *This application applies to any business area in which participants are going to move products amongst each other. It allows for the packaging of several products into one trackable object and then the transferring of ownership/custodianship of said goods.*

 *There are currently 5 Roles/Participants expected by the application, however it can be quickly customized for any supply chain:*
 ```
 (1) Manufacturer: can create products and issue recall notices on products and package products in containers.
 (2) Carrier: accepts ownership of goods upon receiving the physical item as well as package products in containers.
 (3) Warehouse: accepts ownership of goods upon receiving the physical item as well as package products in containers.
 (4) Store: accepts ownership of goods upon receiving the physical item as well as package products in containers.
 (5) Consumer: have a visual on a product once they receive it at the end of the supply chain.
 ```


# Pre-Requisites

See https://docs.corda.net/getting-set-up.html.


# Executing Tests

To run the test suite you can run the command

    **Unix:**

         ./gradlew test

    **Windows:**

         gradlew.bat test


# Nodes

## Configure the nodes

Node configuration can be done via build.gradle in the deployNodes task .....

    node {
            name "CN=<Common Name>,O=<Organization>,L=<Lat/Long/Locality>,C=<Country>"
            p2pPort 10005
            rpcSettings {
                address("localhost:10006")
                adminAddress("localhost:10046")
            }
            sshdPort 2222
            cordapps = [
                    "$project.group:cordapp-contracts-states:$project.version",
                    "$project.group:cordapp-supply-chain:$project.version"
            ]
            rpcUsers = [[ user: "user1", "password": "test", "permissions": ["ALL"]]]
            extraConfig = [ detectPublicIp: false ]

        }

## Building the nodes

**Unix:**

     ./gradlew deployNodes

**Windows:**

     gradlew.bat deployNodes

Note: You'll need to re-run this build step after making any changes to
for these to take effect on the node. Optionally use clean to remove all build files before building,
this will essentially bring up the a clean slate with empty vaults.

## Running the nodes

Once the build finishes, change directories to the folder where the newly
built nodes are located:

     cd build/nodes

The Gradle build script will have created a folder for each node. You'll
see multiple folders, one for each node and a `runnodes` script. You can
run the nodes with:

**Unix:**

     ./runnodes

**Windows:**

    runnodes.bat

# Webservers

## Configure the Webservers

Webservers configuration can be done via application.properties within the resources folder of each nodes Webserver. You
will need one file per node with a unique name (i.e. application-PartyA.properties)


## Build the Webservers

    **Unix:**

         ./gradlew deployWebapps

    **Windows:**

         gradlew.bat deployWebapps

## Running the Webservers

Once the build finishes, change directories to the folder where the newly
built nodes are located:

     cd build/webapps

The Gradle build script will have created a jar for the webserver. You'll
see a single jar file, you can run multiple instances per application.properties previously defined :

**Unix:**

    java -Dspring.profiles.active=PartyA -jar webserver-supply-chain-0.1.jar


**Windows:**

    java -Dspring.profiles.active=PartyA -jar webserver-supply-chain-0.1.jar


# Interacting with the network

## Shell


## Webserver

`clients/src/main/kotlin/com/supplychain/bcc/webserver/` defines a simple Spring webserver that connects to a node via RPC and
allows you to interact with the node over HTTP.

The API endpoints are defined here:

     clients/src/main/kotlin/com/supplychain/bcc//webserver/Controller.kt

To view available endpoints you can hit the swagger docs page at /swagger-ui.html when running or reference confluence page