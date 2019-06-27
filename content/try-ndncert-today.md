Try NDNCERT
============

By Zhiyi Zhang (zhiyi@cs.ucla.edu)

Step 1
------
Download and install NDNCERT.

Step 2
------

Copy past the following configuration file into your `/usr/local/etc/ndncert/client.conf`.

```
{
  "ca-list":
  [
    {
        "ca-prefix": "/ndn/edu/ucla-ndncert",
        "ca-info": "NDN Testbed CA",
        "probe": "email",
        "certificate": "Bv0CzQc4CANuZG4IA2VkdQgMdWNsYS1uZG5jZXJ0CANLRVkICIL06TRSCyYYCARzZWxmCAn9AAABa2duyJMUCRgBAhkEADbugBX9ASYwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDOcoRJBizHOqa/ZVCb+ZLX9jwuOWkMx32oMB2SAxAymqEo34S0r2Bz+0FIfKhPC/w7tJSucQ/b5xVtY5PIkZlFNeQTxc5mTTqNXegWc2AGthGAKbFoBmTf6BCcdfq3eW8u2y05NgSxr441VK/5//ZwnDArzkSqvJx2DkYqKUap202P4pLxvRFtjk8duCe6zq72jS/FdodG1UkmPQm3qMUWW8h/8cNShVeswu7sG/twX9Ve4VqTkpwA78QRbJEmpTMShGaD2etL9dZ+2Q7prx2MP6cszQbisxqOjI5g3s51qn3UbT4KRmyJJ+Hf1qhq9MhgCp3Hzn9uiQcNCVdI/qoNAgMBAAEWWBsBARwpBycIA25kbggDZWR1CAx1Y2xhLW5kbmNlcnQIA0tFWQgIgvTpNFILJhj9AP0m/QD+DzE5NzAwMTAxVDAwMDAwMP0A/w8yMDM5MDYxMlQyMTU0MDIX/QEAgf8eD8cRZylTQdIQhK2M289KpMbEpw948rXe8j+i4lwZm5XoFnvKwqr7Xhr7r2ijYwaCG1i0gT69upzgm/k0TA/zvXUYxwpo+izlF8mMkkFNmX5JwqfO6mXo6wNDJYadJqkx1FVdcRcf5Iyokp5fJcHTwc+FVgkH36hCA9FBCYHmUjcKc/X2ndOnIOAUPQYPLNx+FRDlovRq7lM/BCuT8J5lm61QcUSzXGaRhyGVlFYtEdm6NsLCCz+Gl7telIwVP3qiM6iC/XFvRIJYTbC5eUr/MDs1zTpBhO5L6iWNYGZiCu17kHxffzNMXccTKDrg46EQVgLknTn1F0cUbBmv8w=="
    }
  ]
}
```

Step 3
------

Run `ndncert-client`.