Try NDNCERT
============

By Zhiyi Zhang (zhiyi@cs.ucla.edu)

Step 1
------
Download and install NDNCERT (https://github.com/named-data/ndncert).

```
git clone https://github.com/named-data/ndncert.git
cd ndncert
./waf configure
./waf
./waf install
```

Step 2
------

Download the configuration file (out-of-band bootstrapping).

```
curl -o /usr/local/etc/ndncert/client.conf https://zhiyi-zhang.com/client-conf.txt
```

Step 3
------

Get your NFD connected to the NDN Testbed (If you don't know how, please visit http://named-data.net/doc/NFD/current/INSTALL.html#connecting-to-remote-nfds)

A summary of this process is also shown as follow.
```
nfd-start // start nfd
nfdc face create udp4://spurs.cs.ucla.edu // can be replaced with another URL
nfdc route add /ndn [face_id] // [face_id] is shown in the result of the last command
```

Step 4
------

Run `ndncert-client` command line tool to get a certificate.
Just follow the prompt in the command line tool.

```
ndncert-client
```