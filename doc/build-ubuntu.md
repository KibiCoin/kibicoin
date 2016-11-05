UBUNTU 14.04, 16.04 AND 16.10 BUILD NOTES
====================
Some notes on how to build KibiCoin in Ubuntu 14.04, 16.04 and 16.10.
Instructions are written for virtual machine from osboxes:
http://www.osboxes.org/ubuntu/

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
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=/home/osboxes/kibicoin/db4/ # change osboxes to your username if you are not using osboxes virtual machine
make install
```

Install gcc-5 (only 14.04)
---------------------

```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-5 g++-5
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 1
```

Compile KibiCoin (change osboxes to your username)
---------------------

```bash
cd ~/kibicoin/
./autogen.sh
./configure LDFLAGS="-L/home/osboxes/kibicoin/db4/lib/" CPPFLAGS="-I/home/osboxes/kibicoin/db4/include/ -DBOOST_NO_CXX11_SCOPED_ENUMS" CXXFLAGS="$CXXFLAGS -std=c++0x"
make -s -j2
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
