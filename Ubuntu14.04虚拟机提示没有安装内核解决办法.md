##VBox on 14.04: Kernel driver not installed (rc=-1908)##

```shell
sudo apt-get install linux-headers-generic build-essential dkms
sudo apt-get remove virtualbox-dkms
sudo apt-get install virtualbox-dkms
```

##Documents##
[http://askubuntu.com/questions/498900/vbox-on-14-04-kernel-driver-not-installed-rc-1908](http://askubuntu.com/questions/498900/vbox-on-14-04-kernel-driver-not-installed-rc-1908)