You need to be root user, it works correctly (\ new line)

````bash
virt-install --name ws2 --memory 2048\
--disk path=/var/lib/libvirt/images/ws2.qcow2,size=40,format=qcow2\
--location /var/lib/libvirt/images/Rocky-9.8-x86_64-minimal.iso --network bridge=br0\
--os-variant rocky9 --initrd-inject /var/lib/libvirt/images/rockylinux.cfg\
--extra-args "inst.ks=file:/rockylinux.cfg console=tty0 console=ttyS0,115200n8"\
--graphic none --wait -1
```
On your shell put it and run
