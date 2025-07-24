# Taproot Assets guide

The Taproot assets protocol needs Lightning networks daemon to be running for functionalities such as blockchain monitoring, wallet management etc.

## Start LND

### Testnet
./lnd-debug --lnddir=/data3/.lnd --configfile=/data3/.lnd/lnd.conf --wallet-unlock-password-file=/data3/.lnd/pass.txt

### Mainnet
./lnd-debug --lnddir=/data3/.lnd --configfile=/data3/.lnd/lnd-mainnet.conf --wallet-unlock-password-file=/data3/.lnd/pass.txt

Once the LND daemon has synced you can start the TAPD daemon. 

## Check sync status

### Testnet
lncli --macaroonpath=/data3/.lnd/data/chain/bitcoin/testnet/admin.macaroon --tlscertpath=/data3/.lnd/tls.cert getinfo | grep sync

### Mainnet
lncli --macaroonpath=/data3/.lnd/data/chain/bitcoin/mainnet/admin.macaroon --tlscertpath=/data3/.lnd/tls.cert getinfo | grep sync

## Start TAPD

### Testnet
tapd --network=testnet \
--debuglevel=debug \
--lnd.host=localhost:10009 \
--lnd.macaroonpath=/data3/.lnd/data/chain/bitcoin/testnet/admin.macaroon \
--lnd.tlspath=/data3/.lnd/tls.cert

### Mainnet
tapd --network=mainnet \
--debuglevel=debug \
--lnd.host=localhost:10003 \
--lnd.macaroonpath=/data3/.lnd/data/chain/bitcoin/mainnet/admin.macaroon \
--lnd.tlspath=/data3/.lnd/tls.cert


# Check wallet balance
lncli --lnddir=/data3/.lnd --macaroonpath=/data3/.lnd/data/chain/bitcoin/testnet/admin.macaroon walletbalance
{
    "total_balance":  "0",
    "confirmed_balance":  "0",
    "unconfirmed_balance":  "0",
    "locked_balance":  "0",
    "reserved_balance_anchor_chan":  "0",
    "account_balance":  {
        "default":  {
            "confirmed_balance":  "0",
            "unconfirmed_balance":  "0"
        }
    }
}

# Mint assets

## Mainnet
tapcli --macaroonpath=$HOME/.tapd/data/mainnet/admin.macaroon --network=mainnet  assets mint --type normal --name fantasycoin --supply 100 --meta_bytes "fantastic money"

## Testnet
 tapcli --macaroonpath=$HOME/.tapd/data/testnet/admin.macaroon --network=testnet  assets mint --type normal --name fantasycoin --supply 100 --meta_bytes "fantastic money"

# Finalize 

## Mainnet
tapcli --macaroonpath=$HOME/.tapd/data/mainnet/admin.macaroon assets mint finalize

## Testnet
tapcli --macaroonpath=$HOME/.tapd/data/testnet/admin.macaroon assets mint finalize
