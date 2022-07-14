FROM ethereum/client-go:alltools-stable

RUN mkdir /data
WORKDIR /data

COPY genesis.json .
RUN geth init --datadir /data genesis.json

COPY keystore keystore
COPY password.txt .

COPY static-nodes.json .

COPY miner1.key .
COPY miner2.key .


# geth
EXPOSE 8545
EXPOSE 30303

# bootnode
EXPOSE 30301/udp

# swarm
EXPOSE 8500
EXPOSE 30399