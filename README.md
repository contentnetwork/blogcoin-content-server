# blogcoin-storage-server
Storage server for Loki Service Nodes

Requirements:
* Boost >= 1.66 (for boost.beast)
* OpenSSL >= 1.1.1a (for X25519 curves)
* sodium >= 1.0.16 (for ed25119 to curve25519 conversion)
* Install your boost to /usr/local/src/boost_1_66_0 and install bittoro-storage-server on same user running bittoroed daemon
```
git clone --recursive https://github.com/bittoro/bittoro-storage-server.git
cd bittoro-storage-server
git submodule update --init
mkdir build && cd build
cmake -DDISABLE_SNODE_SIGNATURE=OFF -DCMAKE_BUILD_TYPE=Release -DBOOST_ROOT=/usr/local/src/boost_1_66_0 ..
cmake --build .
./storage-server 0.0.0.0 31080
```

The paths for Boost and OpenSSL can be specified by exporting the variables in the terminal before running `make`:
```
export OPENSSL_ROOT_DIR = ...
export BOOST_ROOT= ...
```

Then using something like Postman (https://www.getpostman.com/) you can hit the API:

# post data
```
HTTP POST http://127.0.0.1/store
body: "hello world"
headers:
- X-Loki-recipient: "mypubkey"
- X-Loki-ttl: "86400"
- X-Loki-timestamp: "1540860811000"
- X-Loki-pow-nonce: "xxxx..."
```
# get data
```
HTTP GET http://127.0.0.1/retrieve
headers:
- X-Loki-recipient: "mypubkey"
- X-Loki-last-hash: "" (optional)
```

# unit tests
```
mkdir build_test
cd build_test
cmake ../unit_test -DBOOST_ROOT="path to boost" -DOPENSSL_ROOT_DIR="path to openssl"
cmake --build .
./Test --log_level=all
```
