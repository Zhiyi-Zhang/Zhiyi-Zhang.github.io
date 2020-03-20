---
layout: default
title:  "Try NDNCET Today"
date:   2020-03-20 14:23:00 -0800
categories: misc
---
# Try NDNCET Today

By Yufeng Zhang and Zhiyi Zhang (zhiyi@cs.ucla.edu)

Last Modified Mar 20 2020

## Instructions

The `NDNCERT` and `repo-ng` are deployed on the ICE-AR server at UCLA, and the server has been connected to the NDN testbed.
All issued certificates will be pushed to `repo-ng`.

To request a certificate, please see the below steps:

### 1) Preparation

make sure `ndn-cxx`, `nfd`, and `Boost` are correctly configured on the machine

### 2) Download NDNCERT

get the latest version of [NDNCERT](https://github.com/named-data/ndncert).

### 3) Compile and Install

switch to the `ndncert` direcory, compile, and install ndncert by running

```bash
cd ndncert
./waf configured
./waf
./waf install
```

If installation fails, try using `sudo ./waf install`.

### 3) Client Config Downloading

switch to `/usr/local/etc/ndncert`, and create `client.conf`

Copy paste the config file from [here](/content/client-conf.txt) to `client.conf`.

### 4) Connect your Node to NDN Testbed

make sure NFD is running, and your machine is connected to the NDN testbed. If not, perform the following operations to start NFD and connect to testbed:

```bash
nfd-start
nfdc face create udp4://spurs.cs.ucla.edu
```

After you run the face creation command, you will see a face ID. Substitute the face ID into the below commands.

```bash
nfdc route add /ndn FACE_ID
```

### 5) Use NDNCERT to get a new NDN certificate

Request a certificate by running ndncert-client on your terminal, and enter the required information.
