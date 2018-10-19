# Orangefs on Ubuntu
Deploy PVFS orangefs on Ubuntu16.04

Add client and server will update later

### Download&unpack
The source code we can get from http://www.orangefs.org/

Then unpack it:
```bash
tar xzf orangefs-2.9.7.tar.gz
```

### Install some prerequisites
```bash
apt install automake build-essential bison flex libattr1 libattr1-dev
apt install -y gcc flex bison libssl-dev libdb-dev linux-source perl make autoconf linux-headers-`uname -r` zip openssl automake autoconf patch g++ 
```

### Compile source code
```bash
mkdir /opt/orangfs
./configure --prefix=/opt/orangefs --enable-shared --with-db-backend=lmdb
make && make install
```

### Insert kernal module
```bash
   lsmod |grep -i orange
   modprobe orangefs
```

### Create a new configuration file
```bash
/opt/orangefs/bin/genconfig /opt/orangfs/etc/orangefs-server.conf
```

## Create /etc/pvfs2tab
```bash
tcp://192.168.56.101:3334/orangefs /mnt/orangefs  pvfs2 defaults 0 0
```

### Initialize orangefs data space (metadata & data)
```bash
/opt/orangefs/sbin/pvfs2-server -f /opt/orangefs/etc/orangefs.conf -a 192.168.56.101
```

### Start service
```bash
/opt/orangefs/sbin/pvfs2-server  /opt/orangefs/etc/orangefs.conf -a 192.168.56.101
```

### Mount orangefs
```bash
mount -t pvfs2 tcp://192.168.56.101:3334/orangefs /mnt/orangefs/
```


### Restart orangefs service
```bash
/opt/orangefs/sbin/pvfs2-stop-all -c /opt/orangefs/etc/orangefs-server.conf
```
if we use root user, we would give a root ssh login permit by modify /etc/sshd.conf

### Test whether configuration is complete
```bash
/opt/orangefs/bin/pvfs2-ping -m /mnt/orangefs
```

### Some installation logs
  243  rm -rf /opt/orangefs/storage/*
  244  ./pvfs2-server -f /opt/orangefs/etc/orangefs.conf -a 192.168.56.101
  245  rm -rf /mnt/orangefs/ #####(Important!!!)
  246  mkdir /mnt/orangefs   #####(Important!!!) mount error always occured, re-create a mnt dir problem solved.
  247  ./pvfs2-server  /opt/orangefs/etc/orangefs.conf -a 192.168.56.101
  248  ps aux |grep pvfs2
  249  ps aux |grep pvfs2-server
  250  mount -t pvfs2 tcp://192.168.56.101:3334/orangefs /mnt/orangefs/

