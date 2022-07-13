# Ethereum PoA private network
A Docker based implementation of blockchain network for testing and development purposes including:
- 1 Bootnode
- 1 Ethereum node
- 2 Ethereum miner nodes
- 2 [Swarm](https://swarm-gateways.net/bzz:/theswarm.eth) nodes (distributed storage platform)
- [Ethereum Network Status Dashboard](https://github.com/goerli/ethstats-server)

Using Proof-of-Authority consensus instead of Proof-of-Work.
New block creates every 5 seconds.

## Prerequisites
You should have installed Docker and Docker Compose in your machine.

```
git clone https://github.com/a1brz/eth-private-network.git
cd eth-private-network

git clone --branch fix/tracer https://github.com/fr0zn/blockscout explorer
```

## Config

- Create two accounts using the same password

```
docker run -it --rm  -v `pwd`/node:/root/.ethereum ethereum/client-go account new
docker run -it --rm  -v `pwd`/node:/root/.ethereum ethereum/client-go account new
```

- Write the account addresses under `.env`, change `ACCOUNT_1` and `ACCOUNT_2` (without `0x`)

- Copy the password under `node/password.txt`

```
echo "password" > `pwd`/config/password.txt
```

- Generate genesis config

```
$ go install github.com/ethereum/go-ethereum/cmd/puppeth@latest # or download Geth & Tools from https://geth.ethereum.org/downloads/
$ puppeth
Please specify a network name to administer (no spaces, hyphens or capital letters please)
> genesis
What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
> 2
What would you like to do? (default = create)
 1. Create new genesis from scratch
 2. Import already existing genesis
> 1
Which consensus engine to use? (default = clique)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
> 2
How many seconds should blocks take? (default = 15)
> 2
Which accounts are allowed to seal? (mandatory at least one)
> 0x # Enter both accounts address created in the previous step
> 0x # Enter both accounts address created in the previous step
Which accounts should be pre-funded? (advisable at least one)
> 0x # Enter both accounts address created in the previous step
> 0x # Enter both accounts address created in the previous step
Should the precompile-addresses (0x1 .. 0xff) be pre-funded with 1 wei? (advisable yes)
> no
Specify your chain/network ID if you want an explicit one (default = random)
>
What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> 2
 1. Modify existing configurations
 2. Export genesis configurations
 3. Remove genesis configuration
> 2
Which folder to save the genesis specs into? (default = current)
  Will create genesis.json, genesis-aleth.json, genesis-harmony.json, genesis-parity.json
> node
```

- Change `NETWORK_ID` under `.env` to match the one from the previous step (it can be found under `node/genesis.json`)



## Run
```bash
docker-compose up --build
```

After all you can find:
- Ethereum Network Stats at [http://localhost:3000](http://localhost:3000)
- Node 1 RPC access at [http://localhost:8545](http://localhost:8545)
- Node 2 (miner) RPC access at [http://localhost:8546](http://localhost:8546)
- Node 3 (miner) RPC access at [http://localhost:8547](http://localhost:8547)
- Swarm Node 1 HTTP access at [http://localhost:8500](http://localhost:8500)
- Swarm Node 2 HTTP access at [http://localhost:8501](http://localhost:8501)
