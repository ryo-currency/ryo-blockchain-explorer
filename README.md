# Ryo Blockchain Explorer

Forked from [Onion Monero Blockchain Explorer](https://github.com/moneroexamples/onion-monero-blockchain-explorer)

Full featured Ryo blockchain explorer for privacy-oriented users:

 - Does not use JavaScript,
 - Does not have images which might be used for [cookieless tracking](http://lucb1e.com/rp/cookielesscookies/),
 - Does not track users activates through google analytics,
 - Open source,
 - Supports Ryo testnet nor stagenet networks,
 - Full featured JSON API.


Libraries:

 - [crow](https://github.com/ipkn/crow) - C++ micro web framework
 - [mstch](https://github.com/no1msd/mstch) - C++ {{mustache}} templates
 - [json](https://github.com/nlohmann/json) - JSON for Modern C++
 - [fmt](https://github.com/fmtlib/fmt) - Small, safe and fast string formatting library

## Explorer hosts

 - [https://explorer.ryo-currency.com/](https://explorer.ryo-currency.com/) - Official Explorer
 - [https://explorer.ryoblocks.com/](https://explorer.ryoblocks.com/) - RyoBlocks Explorer
 - [https://tnexp.ryo-currency.com/](https://tnexp.ryo-currency.com/) - Testnet Explorer

## Ryo Blockchain Explorer features

The key features of the Ryo Blockchain Explorer are:

 - no cookies, no web analytics trackers, no images,
 - by default no JavaScript, but can be enabled for client side decoding and proving transactions,
 - open sourced,
 - made fully in C++,
 - showing encrypted payments ID,
 - showing ring signatures,
 - showing transaction extra field,
 - showing public components of Ryo addresses,
 - decoding which outputs and mixins belong to the given Ryo address and viewkey,
 - can prove that you send Ryo to someone,
 - detailed information about ring members, such as, their age, timescale and their ring sizes,
 - showing number of amount output indices,
 - support Ryo testnet and stagnet networks,
 - tx checker and pusher for online pushing of transactions,
 - estimate possible spendings based on address and viewkey,
 - can provide total amount of all miner fees,
 - decoding encrypted payment id,
 - decoding outputs and proving txs sent to sub-address.


## Compilation on Ubuntu 16.04/18.04

##### Compile latest Ryo development version

Download and compile recent Ryo into your home folder:

```bash
# first install ryo dependecines
sudo apt update

sudo apt install build-essential cmake pkg-config libboost-all-dev libssl-dev libzmq3-dev libunbound-dev libsodium-dev libminiupnpc-dev libunwind8-dev liblzma-dev libreadline6-dev libldns-dev libexpat1-dev doxygen graphviz libpcsclite-dev

# go to home folder
cd ~

git clone --recursive https://github.com/ryo-currency/ryo-currency.git

cd ryo-currency/

make
```

##### Compile and run the explorer

Once the Ryo is compiled, the explorer can be downloaded and compiled
as follows:

```bash
# install explorer dependecines
sudo apt install libasio-dev libcurl4-openssl-dev

# go to home folder if still in ~/ryo-currency
cd ~

# download the source code
git clone https://github.com/ryo-currency/ryo-blockchain-explorer.git

# enter the downloaded sourced code folder
cd ryo-blockchain-explorer

# make a build folder and enter it
mkdir build && cd build

# create the makefile
cmake -DMONERO_DIR=~/ryo-currency ..

# also can build with ASAN (sanitizers), for example
# cmake -DSANITIZE_ADDRESS=On ..

# compile
make
```


To run it:
```
./xmrblocks -b /home/user/.ryo/lmdb02/
```

Example output:

```bash
user@arch:~/ryo-blockchain-explorer/build$ ./xmrblocks -b /home/user/.ryo/lmdb02
Staring in non-ssl mode
(2018-07-27 01:06:02) [INFO    ] Crow/0.1 server is running at 127.0.0.1:8081 using 4 threads
```

Go to your browser: http://127.0.0.1:8081

## The explorer's command line options

```
xmrblocks, Ryo Blockchain Explorer:
  -h [ --help ] [=arg(=1)] (=0)         produce help message
  -t [ --testnet ] [=arg(=1)] (=0)      use testnet blockchain
  -s [ --stagenet ] [=arg(=1)] (=0)     use stagenet blockchain
  --enable-pusher [=arg(=1)] (=0)       enable signed transaction pusher
  --enable-mixin-details [=arg(=1)] (=0)
                                        enable mixin details for key images,
                                        e.g., timescale, mixin of mixins, in tx
                                        context
  --enable-key-image-checker [=arg(=1)] (=0)
                                        enable key images file checker
  --enable-output-key-checker [=arg(=1)] (=0)
                                        enable outputs key file checker
  --enable-json-api [=arg(=1)] (=1)     enable JSON REST api
  --enable-tx-cache [=arg(=1)] (=0)     enable caching of transaction details
  --show-cache-times [=arg(=1)] (=0)    show times of getting data from cache
                                        vs no cache
  --enable-block-cache [=arg(=1)] (=0)  enable caching of block details
  --enable-js [=arg(=1)] (=0)           enable checking outputs and proving txs
                                        using JavaScript on client side
  --enable-autorefresh-option [=arg(=1)] (=0)
                                        enable users to have the index page on
                                        autorefresh
  --enable-emission-monitor [=arg(=1)] (=0)
                                        enable Ryo total emission monitoring
                                        thread
  -p [ --port ] arg (=8081)             default explorer port
  --testnet-url arg                     you can specify testnet url, if you run
                                        it on mainnet or stagenet. link will
                                        show on front page to testnet explorer
  --stagenet-url arg                    you can specify stagenet url, if you
                                        run it on mainnet or testnet. link will
                                        show on front page to stagenet explorer
  --mainnet-url arg                     you can specify mainnet url, if you run
                                        it on testnet or stagenet. link will
                                        show on front page to mainnet explorer
  --no-blocks-on-index arg (=10)        number of last blocks to be shown on
                                        index page
  --mempool-info-timeout arg (=5000)    maximum time, in milliseconds, to wait
                                        for mempool data for the front page
  --mempool-refresh-time arg (=5)       time, in seconds, for each refresh of
                                        mempool state
  -b [ --bc-path ] arg                  path to lmdb folder of the blockchain,
                                        e.g., ~/.ryo/lmdb02
  --ssl-crt-file arg                    path to crt file for ssl (https)
                                        functionality
  --ssl-key-file arg                    path to key file for ssl (https)
                                        functionality
  -d [ --deamon-url ] arg (=http:://127.0.0.1:12211)
                                        Ryo deamon url
```

## Enable Ryo emission

Obtaining current Ryo emission amount is not straight forward. Thus, by default it is
disabled. To enable it use `--enable-emission-monitor` flag, e.g.,


```bash
xmrblocks --enable-emission-monitor
```

This flag will enable emission monitoring thread. When started, the thread
 will initially scan the entire blockchain, and calculate the cumulative emission based on each block.
Since it is a separate thread, the explorer will work as usual during this time.
Every 10000 blocks, the thread will save current emission in a file, by default,
 in `~/.ryo/lmdb/emission_amount.txt`. For testnet or stagenet networks,
 it is `~/.ryo/testnet/lmdb/emission_amount.txt` or `~/.ryo/stagenet/lmdb/emission_amount.txt`. This file is used so that we don't
 need to rescan entire blockchain whenever the explorer is restarted. When the
 explorer restarts, the thread will first check if `~/.ryo/lmdb/emission_amount.txt`
 is present, read its values, and continue from there if possible. Subsequently, only the initial
 use of the tread is time consuming. Once the thread scans the entire blockchain, it updates
 the emission amount using new blocks as they come. Since the explorer writes this file, there can
 be only one instance of it running for mainnet, testnet and stagenet. Thus, for example, you cant have
 two explorers for mainnet
 running at the same time, as they will be trying to write and read the same file at the same time,
 leading to unexpected results. Off course having one instance for mainnet and one instance for testnet
 is fine, as they write to different files.

 When the emission monitor is enabled, information about current emission of coinbase and fees is
 displayed on the front page, e.g., :

```
Ryo emission (fees) is 14485540.430 (52545.373) as of 1313448 block
```

The values given, can be checked using Ryo daemon's  `print_coinbase_tx_sum` command.
For example, for the above example: `print_coinbase_tx_sum 0 1313449`.

To disable the monitor, simply restart the explorer without `--enable-emission-monitor` flag.

Note: The pre-mined coins have been frozen/burned in commit [c3a3cb6](https://github.com/ryo-currency/ryo-emergency/commit/c3a3cb620488e88be7c52e017072261a3063b872)/ [blockchain_db/blockchain_db.cpp#L250-L258](https://github.com/ryo-currency/ryo-emergency/blob/c3a3cb620488e88be7c52e017072261a3063b872/src/blockchain_db/blockchain_db.cpp#L250-L258) and a modification has been made to the explorer to remove 8,700,000 coins from circulation.


## Enable JavaScript for decoding proving transactions

By default, decoding and proving tx's outputs are done on the server side. To do this on the client side
(private view and tx keys are not send to the server) JavaScript-based decoding can be enabled:

```
xmrblocks --enable-js
```

## Enable SSL (https)

By default, the explorer does not use ssl. But it has such a functionality.

As an example, you can generate your own ssl certificates as follows:

```bash
cd /tmp # example folder
openssl genrsa -out server.key 1024
openssl req -new -key server.key -out server.csr
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
```

Having the `crt` and `key` files, run `xmrblocks` in the following way:

```bash
./xmrblocks --ssl-crt-file=/tmp/server.crt --ssl-key-file=/tmp/server.key
```

Note: Because we generated our own certificate, modern browsers will complain
about it as they can't verify the signatures against any third party. So probably
for any practical use need to have properly issued ssl certificates. Alternatively,
use an SSL terminator such as nginx.

## JSON API

The explorer has JSON api. For the API, it uses conventions defined by [JSend](https://labs.omniti.com/labs/jsend).
By default the api is disabled. To enable it, use `--enable-json-api` flag, e.g.,

```
./xmrblocks --enable-json-api
```

#### api/transaction/<tx_hash>

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/transaction/4307e5619b29dcc1d9cdf4a52b3d0717bcbab0efc06d88fb79a1e0e33594e6ec"
```

Partial results shown:

```json
{
   "data":{
      "block_height":156844,
      "coinbase":true,
      "confirmations":4,
      "current_height":156848,
      "extra":"0190c6a55fe6d77033f225291b36d22b700264d9c3f7c3648516f73f6470216015021039303031000000020000001049d4e900",
      "inputs":null,
      "mixin":0,
      "outputs":[
         {  
            "amount":41978500000,
            "public_key":"9935ac366ebd6fb6aee84581d9027dd0498ff6761a470bfaa8e48ff429788e22"
         }
      ],
      "payment_id":"",
      "payment_id8":"",
      "rct_type":0,
      "timestamp":1532649166,
      "timestamp_utc":"2018-07-26 23:52:46",
      "tx_fee":0,
      "tx_hash":"4307e5619b29dcc1d9cdf4a52b3d0717bcbab0efc06d88fb79a1e0e33594e6ec",
      "tx_size":102,
      "tx_version":3,
      "xmr_inputs":0,
      "xmr_outputs":41978500000
   },
   "status":"success"
}
```

---

#### api/transactions

Transactions in last 25 blocks

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/transactions"
```

Partial results shown:

```json
{
   "data":{
      "blocks":[
         {
            "age":"00:05:41",
            "hash":"e3ff8c52516b8389888ee742d7f18448e3760199560f24ebd80092e46019fa9c",
            "height":156848,
            "size":28268.0,
            "timestamp":1532649512,
            "timestamp_utc":"2018-07-26 23:58:32",
            "txs":[
               {
                  "coinbase":true,
                  "extra":"01c9417029a41208a7142bf3a458935403328b34ca54a6c8b8a2ecf661043844000211af08e9cf0732704a000000000000000000",
                  "mixin":0,
                  "payment_id":"",
                  "payment_id8":"",
                  "rct_type":0,
                  "tx_fee":0,
                  "tx_hash":"5ace475abe36a574ee0d73e2c6aa5ba37473361934ed1b36981f17cbd8c6ecb4",
                  "tx_size":103,
                  "tx_version":3,
                  "xmr_inputs":0,
                  "xmr_outputs":41999000000
               },
               {
                  "coinbase":false,
                  "extra":"0181a8586bb9e3f98e04b80496632a9fea2afa59fb5e3c844b1ab28182f833e053",
                  "mixin":13,
                  "payment_id":"",
                  "payment_id8":"",
                  "rct_type":1,
                  "tx_fee":14000000,
                  "tx_hash":"242b5bca09edf614a6440a183c949934dbc1de82176bc2103141be7aed39ae32",
                  "tx_size":13586,
                  "tx_version":3,
                  "xmr_inputs":0,
                  "xmr_outputs":0
               },
               {
                  "coinbase":false,
                  "extra":"01622d73b89759ab0ba0d9fb211fd430ada1b5958eea6a14b5aa08f0cd0a1def1c",
                  "mixin":13,
                  "payment_id":"",
                  "payment_id8":"",
                  "rct_type":2,
                  "tx_fee":15000000,
                  "tx_hash":"01cf17ce53e132f1a987cb1b4dbfe51d13a163425d2202744c624166514746d9",
                  "tx_size":14579,
                  "tx_version":3,
                  "xmr_inputs":0,
                  "xmr_outputs":0
               }
            ]
         },
      ],
      "current_height":156849,
      "limit":25,
      "page":0,
      "total_page_no":6273
   },
   "status":"success"
}
```

---

#### api/transactions?page=<page_no>&limit=<tx_per_page>

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/transactions?page=2&limit=10"
```

Result analogical to the one above.

---

#### api/block/<block_number|block_hash>

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/block/150000"
```

Partial results shown:

```json
{
   "data":{
      "block_height":150000,
      "current_height":156849,
      "hash":"debc58ac64fc8dc7ae35b0b8782ae33a0fe5855dc85930bc52b8fec3c1aee94a",
      "size":422899,
      "timestamp":1530990884,
      "timestamp_utc":"2018-07-07 19:14:44",
      "txs":[
         {
            "coinbase":true,
            "extra":"01e10420c2c5461ace855ffc8aa4a8256e784e873c325d71e798812ac90c35c19b020816d80f5c00000000",
            "mixin":0,
            "payment_id":"",
            "payment_id8":"",
            "rct_type":0,
            "tx_fee":0,
            "tx_hash":"2743243d3eb70b88baa26f70dd20c606470c5995de194ad9d0d4acf83d5ab761",
            "tx_size":94,
            "tx_version":3,
            "xmr_inputs":0,
            "xmr_outputs":42187500000
         },
      ]
   },
   "status":"success"
}
```

---

#### api/mempool

Return all txs in the mempool.

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/mempool"
```

Partial results shown:

```json
{
   "data":{
      "limit":100000000,
      "page":0,
      "total_page_no":0,
      "txs":[
         {
            "coinbase":false,
            "extra":"0221000140bc7298830abaeb711b7214acb45c431db86c52da40f739fb6908f5dbe596019d29ab0f8b846889d1971d8d7915baab2b2335c4105bcdda1517544f2c4aa086",
            "mixin":13,
            "payment_id":"0140bc7298830abaeb711b7214acb45c431db86c52da40f739fb6908f5dbe596",
            "payment_id8":"",
            "rct_type":2,
            "timestamp":0,
            "timestamp_utc":"1970-01-01 00:00:00",
            "tx_fee":8000000,
            "tx_hash":"8f00e3746c7fe1e50bd72da69e00449b53586fbd793ed392a63c283b9fbf7c49",
            "tx_size":15584,
            "tx_version":3,
            "xmr_inputs":0,
            "xmr_outputs":0
         },
      ],
      "txs_no":3
   },
   "status":"success"
}
```

Limit of 100000000 is just default value above to ensure that all mempool txs are fetched
if no specific limit given.

---

#### api/mempool?limit=<no_of_top_txs>

Return number of newest mempool txs, e.g., only 10.

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/mempool?limit=10"
```

Result analogical to the one above.

---

#### api/search/<block_number|tx_hash|block_hash>

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/search/150000"
```

Partial results shown:

```json
{
   "data":{
      "block_height":150000,
      "current_height":156850,
      "hash":"debc58ac64fc8dc7ae35b0b8782ae33a0fe5855dc85930bc52b8fec3c1aee94a",
      "size":422899,
      "timestamp":1530990884,
      "timestamp_utc":"2018-07-07 19:14:44",
      "title":"block",
      "txs":[
         {
            "coinbase":true,
            "extra":"01e10420c2c5461ace855ffc8aa4a8256e784e873c325d71e798812ac90c35c19b020816d80f5c00000000",
            "mixin":0,
            "payment_id":"",
            "payment_id8":"",
            "rct_type":0,
            "tx_fee":0,
            "tx_hash":"2743243d3eb70b88baa26f70dd20c606470c5995de194ad9d0d4acf83d5ab761",
            "tx_size":94,
            "tx_version":3,
            "xmr_inputs":0,
            "xmr_outputs":42187500000
         },
      ]
   },
   "status":"success"
}
```

---

#### api/outputs?txhash=<tx_hash>&address=<address>&viewkey=<viewkey>&txprove=<0|1>

For `txprove=0` we check which outputs belong to given address and corresponding viewkey.
For `txprove=1` we use to prove to the recipient that we sent them founds.
For this, we use recipient's address and our tx private key as a viewkey value,
 i.e., `viewkey=<tx_private_key>`

Checking outputs:

```bash
# we use here official Ryo project's donation address as an example
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/outputs?txhash=17049bc5f2d9fbca1ce8dae443bbbbed2fc02f1ee003ffdd0571996905faa831&address=RYoLshW4LVn8TurtCrZzAd79FURtPHb93ATRYch3JpSDgvPRubk1VeEKg33sVifjASf7NzZtgQ6urVAAAKDKDtmUiDraNbdDvyk&viewkey=f359631075708155cc3d92a32b75a7d02a5dcf27756707b47a2b31b21c389501&txprove=0"
```

```json
{
  "data": {
    "address": "RYoLshW4LVn8TurtCrZzAd79FURtPHb93ATRYch3JpSDgvPRubk1VeEKg33sVifjASf7NzZtgQ6urVAAAKDKDtmUiDraNbdDvyk",
    "outputs": [
      {
        "amount": 34980000000000,
        "match": true,
        "output_idx": 0,
        "output_pubkey": "35d7200229e725c2bce0da3a2f20ef0720d242ecf88bfcb71eff2025c2501fdb"
      },
      {
        "amount": 0,
        "match": false,
        "output_idx": 1,
        "output_pubkey": "44efccab9f9b42e83c12da7988785d6c4eb3ec6e7aa2ae1234e2f0f7cb9ed6dd"
      }
    ],
    "tx_hash": "17049bc5f2d9fbca1ce8dae443bbbbed2fc02f1ee003ffdd0571996905faa831",
    "tx_prove": false,
    "viewkey": "f359631075708155cc3d92a32b75a7d02a5dcf27756707b47a2b31b21c389501"
  },
  "status": "success"
}
```

Proving transfer:

We use recipient's address (i.e. not our address from which we sent Ryo to recipient).
For the viewkey, we use `tx_private_key` (although the GET variable is still called `viewkey`) that we obtained by sending this txs.

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/outputs?txhash=94782a8c0aa8d8768afa0c040ef0544b63eb5148ca971a024ac402cad313d3b3&address=RYoLshW4LVn8TurtCrZzAd79FURtPHb93ATRYch3JpSDgvPRubk1VeEKg33sVifjASf7NzZtgQ6urVAAAKDKDtmUiDraNbdDvyk&viewkey=e94b5bfc599d2f741d6f07e3ab2a83f915e96fb374dfb2cd3dbe730e34ecb40b&txprove=1"
```

```json
{
  "data": {
    "address": "RYoLshW4LVn8TurtCrZzAd79FURtPHb93ATRYch3JpSDgvPRubk1VeEKg33sVifjASf7NzZtgQ6urVAAAKDKDtmUiDraNbdDvyk",
    "outputs": [
      {
        "amount": 1000000000000,
        "match": true,
        "output_idx": 0,
        "output_pubkey": "c1bf4dd020b5f0ab70bd672d2f9e800ea7b8ab108b080825c1d6cfc0b7f7ee00"
      },
      {
        "amount": 0,
        "match": false,
        "output_idx": 1,
        "output_pubkey": "8c61fae6ada2a103565dfdd307c7145b2479ddb1dab1eaadfa6c34db65d189d5"
      }
    ],
    "tx_hash": "94782a8c0aa8d8768afa0c040ef0544b63eb5148ca971a024ac402cad313d3b3",
    "tx_prove": true,
    "viewkey": "e94b5bfc599d2f741d6f07e3ab2a83f915e96fb374dfb2cd3dbe730e34ecb40b"
  },
  "status": "success"
}
```

Result analogical to the one above.

---

#### api/networkinfo

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/networkinfo"
```

```json
{
   "data":{
      "alt_blocks_count":9,
      "block_size_limit":491520,
      "block_size_median":14191,
      "cumulative_difficulty":284889040816739,
      "current":true,
      "current_hf_version":4,
      "difficulty":1085277793,
      "fee_per_kb":500000,
      "grey_peerlist_size":0,
      "hash_rate":4521990,
      "height":156853,
      "incoming_connections_count":12,
      "outgoing_connections_count":1,
      "stagenet":false,
      "start_time":1531440507,
      "status":true,
      "target":240,
      "target_height":156824,
      "testnet":false,
      "top_block_hash":"f0eb0e43acbd75c040279f847129573aa0d5e43e5d0a13f601ccf04c2fd689a3",
      "tx_count":433613,
      "tx_pool_size":1,
      "tx_pool_size_kbytes":15582,
      "white_peerlist_size":1000
   },
   "status":"success"
}
```

---

#### api/outputsblocks

Search for our outputs in last few blocks (up to 5 blocks), using provided address and viewkey.

```bash
curl  -w "\n" -X GET https://explorer.ryo-currency.com/api/outputsblocks?address=RYoLshW4LVn8TurtCrZzAd79FURtPHb93ATRYch3JpSDgvPRubk1VeEKg33sVifjASf7NzZtgQ6urVAAAKDKDtmUiDraNbdDvyk&viewkey=807079280293998634d66e745562edaaca45c0a75c8290603578b54e9397e90a&limit=5&mempool=1
```

Example result:

```json
{
  "data": {
    "address": "0182d5be0f708cecf2b6f9889738bde5c930fad846d5b530e021afd1ae7e24a687ad50af3a5d38896655669079ad0163b4a369f6c852cc816dace5fc7792b72f",
    "height": 960526,
    "limit": "5",
    "mempool": true,
    "outputs": [
      {
        "amount": 33000000000000,
        "block_no": 0,
        "in_mempool": true,
        "output_idx": 1,
        "output_pubkey": "2417b24fc99b2cbd9459278b532b37f15eab6b09bbfc44f9d17e15cd25d5b44f",
        "payment_id": "",
        "tx_hash": "9233708004c51d15f44e86ac1a3b99582ed2bede4aaac6e2dd71424a9147b06f"
      },
      {
        "amount": 2000000000000,
        "block_no": 960525,
        "in_mempool": false,
        "output_idx": 0,
        "output_pubkey": "9984101f5471dda461f091962f1f970b122d4469077aed6b978a910dc3ed4576",
        "payment_id": "0000000000000055",
        "tx_hash": "37825d0feb2e96cd10fa9ec0b990ac2e97d2648c0f23e4f7d68d2298996acefd"
      },
      {
        "amount": 96947454120000,
        "block_no": 960525,
        "in_mempool": false,
        "output_idx": 1,
        "output_pubkey": "e4bded8e2a9ec4d41682a34d0a37596ec62742b28e74b897fcc00a47fcaa8629",
        "payment_id": "0000000000000000000000000000000000000000000000000000000000001234",
        "tx_hash": "4fad5f2bdb6dbd7efc2ce7efa3dd20edbd2a91640ce35e54c6887f0ee5a1a679"
      }
    ],
    "viewkey": "807079280293998634d66e745562edaaca45c0a75c8290603578b54e9397e90a"
  },
  "status": "success"
}
```

---

#### api/emission

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/emission"
```

```json
{
   "data":{
      "blk_no":156852,
      "coinbase":14364698360000000,
      "fee":5547807947200
   },
   "status":"success"
}
```

---

#### api/version

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/version"
```

```json
{
   "data":{
      "api":65536,
      "blockchain_height":156853,
      "git_branch_name":"templates",
      "last_git_commit_date":"2018-07-02",
      "last_git_commit_hash":"70be873",
      "monero_version_full":"0.2.0.0-fb56bed"
   },
   "status":"success"
}
```

api number is store as `uint32_t`. In this case `65536` represents
major version 1 and minor version 0.
In JavaScript to get these numbers, one can do as follows:

```javascript
var api_major = response.data.api >> 16;
var api_minor = response.data.api & 0xffff;
```

---

#### api/rawblock/<block_number|block_hash>

Return raw json block data, as represented in Ryo.

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/rawblock/150000"
```

Example result not shown.

---

#### api/rawtransaction/<tx_hash>

Return raw json tx data, as represented in Ryo.

```bash
curl  -w "\n" -X GET "https://explorer.ryo-currency.com/api/rawtransaction/4307e5619b29dcc1d9cdf4a52b3d0717bcbab0efc06d88fb79a1e0e33594e6ec"
```

Example result not shown.
