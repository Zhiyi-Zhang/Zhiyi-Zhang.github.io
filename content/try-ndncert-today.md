Try NDNCERT Today
=================

The `NDNCERT` and `repo-ng` are deployed on the ICE-AR server at UCLA, and the server has been connected to the NDN testbed.
All issued certificates will be pushed to `repo-ng`.

To request a certificate, please see the below steps:

0) make sure `ndn-cxx`, `nfd`, and `Boost` are correctly configured on the machine

1) get the latest version of NDNCERT on https://github.com/named-data/ndncert

2) switch to the `ndncert` direcory, compile, and install ndncert by running

```
cd ndncert
./waf configured
./waf
./waf install
```

If installation fails, try using `sudo ./waf install`.

3) switch to `/usr/local/etc/ndncert`, and create `client.conf`

use the following configuration for this file

```json
{
  "ca-list":
  [
        {
        "ca-prefix": "/ndn/edu/ucla/yufeng",
        "ca-info": "UCLA CA in NDN Testbed",
        "probe": "email",
        "certificate":
        "Bv0DfQc2CANuZG4IA2VkdQgEdWNsYQgGeXVmZW5nCANLRVkICHelnivON8k8CAJOQQgJ/QAAAWx3XLP1FAkYAQIZBAA27oAV/QEmMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApvMgYc/PTZDcgKC3BAQ5zEs/B09pJ8TpR41BjGJe1KIr0P0MGZlCW1ZKbsV8y+gZKUZefVwqwfHUJlto/OpQgxm2oZED/f/m4F80KIRzpGi7sF0PvOiZErXEUAJzTrhb5SFzQo9P9n4Zi7uptltFPkpTdZrSKiKfW7ejn5woXoUgjC1EnMQAx+UFPjqbd3C1t5SlmfYtRFiaekF2LN+rkb1AnQ99lisSOwGdwbbyyxrcbclI5DxH9xrbAswHnn26grEGczAzM940Ksvsyd0tfc2EQirHc+IYWHoESMySzB6bJ0Q1GAG6INqklSAfOCz0upKzmyOcTE9yxYblsRmv9QIDAQABFv0BCBsBARwhBx8IA25kbggDZWR1CAR1Y2xhCANLRVkICNIoXYQ3ivN6/QD9Jv0A/g8yMDE5MDgwOFQxNzEwNTn9AP8PMjAyMDA4MDhUMTcxMDU5/QECtP0CAA/9AgEHYWR2aXNvcv0CAgD9AgAc/QIBBWVtYWls/QICD3l1ZmVuZ0B1Y2xhLmVkdf0CABz9AgEIZnVsbG5hbWX9AgIMWXVmZW5nIFpoYW5n/QIADf0CAQVncm91cP0CAgD9AgAP/QIBB2hvbWV1cmz9AgIA/QIAOf0CAQxvcmdhbml6YXRpb279AgIlVW5pdmVyc2l0eSBvZiBDYWxpZm9ybmlhLCBMb3MgQW5nZWxlcxf9AQBdzD9rcuhnba7pHA/TSptn+qhCq10Y5cPMDmk8fhUxWSJdJ/BAml4gI539uIUIy6g+ZbpxPTBR4YwJSsP5FWFENIpGcB3spZrBnDCzGKY2iNTODaVi4dHv8UAcVaN5cNUVbvr8KvZyzTHN4UYJTu1NAnw3J3SleOF+8Rfujv4rRyq+5ydqw36aZc5Dkp4oYOXhkMlvZijUy/IIuetVvEJxvQsixw4p+ZrANCqow5glHKI4B/WbQJmjfyx+3TJ0QXnpV3sL9trNqEimTYY1P+VTl/hOVsC4E7knNcF3zzGFaBPyI2Q511N3nuHVtrDAnPjnEddNSoF4NRDILg6iCSrJ"
        }
  ]
}
```

4) make sure NFD is running, and your machine is connected to the NDN testbed. If not, perform the following operations to start NFD and connect to testbed:

```bash
nfd-start
nfdc face create udp4://spurs.cs.ucla.edu
```

After you run the face creation command, you will see a face ID. Substitute the face ID into the below commands.

```
nfdc route add /ndn FACE_ID
```

5) Request a certificate by running ndncert-client on your terminal, and enter the required information.

Try NDNCERT (Old version)
=========================

By Zhiyi Zhang (zhiyi@cs.ucla.edu)

Last modified: Jun 27 2019

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
curl -o /usr/local/etc/ndncert/client.conf https://zhiyi-zhang.com/content/client-conf.txt
```

Step 3
------

Get your NFD connected to the NDN Testbed (If you don't know how, please visit http://named-data.net/doc/NFD/current/INSTALL.html#connecting-to-remote-nfds)

A summary of this process is also shown as follow.

Currently, the NDNCERT test server is only available through UCLA's NDN forwarder (`udp4://spurs.cs.ucla.edu`), so please use `udp4://spurs.cs.ucla.edu` in the route to NDNCERT test server.
The CA is under the prefix `/ndn/edu/ucla-ndncert/CA`.

```
nfd-start # start nfd
nfdc face create udp4://spurs.cs.ucla.edu # create a face to UCLA site
nfdc route add /ndn [face_id] # [face_id] is shown in the result of the last command
```


Step 4
------

Run `ndncert-client` command line tool to get a certificate.
Just follow the prompt in the command line tool.

```
ndncert-client
```