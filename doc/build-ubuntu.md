UBUNTU 16.10 BUILD NOTES
====================
Some notes on how to build KibiCoin in Ubuntu 16.10.

Instructions written for virtual machine from osboxes:
http://www.osboxes.org/ubuntu/#ubuntu-16-10-vmware

Prepare system
---------------------

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev libboost-all-dev libqt5gui5 libqt5core5a libqt5core5a:i386 libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler libqrencode-dev libminiupnpc-dev libevent-dev git libdb++-dev
```

Prepare kibicoin folder
---------------------

```bash
cd ~
git clone https://github.com/KibiCoin/kibicoin.git
mkdir ./kibicoin/db4
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
tar -xzvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix/
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=/home/osboxes/kibicoin/db4/
make install
```

Compile KibiCoin
---------------------

```bash
cd ~/kibicoin/
./autogen.sh
./configure LDFLAGS="-L/home/osboxes/kibicoin/db4/lib/" CPPFLAGS="-I/home/osboxes/kibicoin/db4/include/"
make -s -j6
```

Run KibiCoin deamon, CLI and QT
---------------------

```bash
./src/kibicoind
./src/kibicoin-cli getinfo
./src/qt/kibicoin-qt
```

System requirements
--------------------

C++ compilers are memory-hungry. It is recommended to have at least 1 GB of
memory available when compiling KibiCoin Core. With 512MB of memory or less
compilation will take much longer due to swap thrashing.
