# Ceph backup
Web panel for managing backup routines in a Ceph cluster

## Up & running the cluster with Vagrant
Please follow instruction from this repository https://github.com/carmstrong/multinode-ceph-vagrant


Modified Ceph deployment

```
vagrant@ceph-admin:~/test-cluster$ ssh ceph-server-1 "sudo mkdir /var/local/osd0 && sudo chown ceph:ceph /var/local/osd0"
vagrant@ceph-admin:~/test-cluster$ ssh ceph-server-2 "sudo mkdir /var/local/osd1 && sudo chown ceph:ceph /var/local/osd1"
vagrant@ceph-admin:~/test-cluster$ ssh ceph-server-3 "sudo mkdir /var/local/osd2 && sudo chown ceph:ceph /var/local/osd2"
vagrant@ceph-admin:~/test-cluster$ ceph-deploy osd prepare ceph-server-1:/var/local/osd0 ceph-server-2:/var/local/osd1 ceph-server-3:/var/local/osd2
vagrant@ceph-admin:~/test-cluster$ ceph-deploy osd activate ceph-server-1:/var/local/osd0 ceph-server-2:/var/local/osd1 ceph-server-3:/var/local/osd2
```

Modified block device creation, found in issue https://github.com/carmstrong/multinode-ceph-vagrant/issues/17

```
$ vagrant ssh ceph-client
vagrant@ceph-client:~$ sudo rbd create foo --size 4096 -m ceph-server-1
vagrant@ceph-client:~$ sudo rbd create foo --size 4096 --image-feature layering -m ceph-server-1
vagrant@ceph-client:~$ sudo mkfs.ext4 -m0 /dev/rbd/rbd/foo
vagrant@ceph-client:~$ sudo mkdir /mnt/ceph-block-device
vagrant@ceph-client:~$ sudo mount /dev/rbd/rbd/foo /mnt/ceph-block-device
```

Generate random data file

```
sudo dd if=/dev/urandom of=tempfile1 bs=1M count=1000
```


## Ceph maintenance command

Check status of monitor on ceph-server-3 node

```
vagrant@ceph-server-3:~$ sudo systemctl status ceph-mon@ceph-server-3
```

Start monitor

```
vagrant@ceph-server-3:~$ sudo systemctl start ceph-mon@ceph-server-3
```
