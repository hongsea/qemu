https://fosspost.org/tutorials/use-qemu-test-operating-systems-distributions

modprobe tun
sudo mknod /dev/net/tun c 10 200
# create image
```
$ qemu-img create -f qcow2 testing-images.img 20G
```
#boot from iso
```
$ sudo qemu-system-x86_64 -m 1024 -device qemu-xhci -boot d -enable-kvm -smp 3 -net nic -net user -hda testing-images.img -cdrom /run/media/pionux/8EC2-F66B/archlinux-2019.06.01-x86_64.iso
```
#run image
```
$ qemu-system-x86_64 -m 1024 -boot d -enable-kvm -smp 3 -net nic -net user -hda testing-images.img
```
#driver
```
$ sudo  modprobe vhost_net
```
#run image with usb driver
```
$ sudo qemu-system-x86_64 -m 1024 -soundhw hda -device qemu-xhci -boot d -enable-kvm -smp 3 -net nic -net user -hda testing-images.img
```

#bridge network
```
sudo qemu-system-x86_64 -m 1024 -soundhw hda -device qemu-xhci -boot d -enable-kvm -smp 3 -net nic -net bridge,br=br0 -hda testing-images.img
```
#Usb audio driver 
```
sudo qemu-system-x86_64 -m 3072 -device qemu-xhci -soundhw hda -device usb-ehci,id=ehci -device usb-host,bus=ehci.0,vendorid=0x125f,productid=0xdb8a -boot d -enable-kvm -smp 3 -net nic -net user -hda window10.img

    -usbdevice tablet \
    -usbdevice host:058f:6387
```
====================================================
```
echo "allow all" | sudo tee /etc/qemu/${USER}.conf
echo "include /etc/qemu/${USER}.conf" | sudo tee --append /etc/qemu/bridge.conf
sudo chown root:${USER} /etc/qemu/${USER}.conf
sudo chmod 640 /etc/qemu/${USER}.conf
```

```
qemu-system-x86_64 -enable-kvm -hda testing-images.img -m 2048 -k en-us -usbdevice tablet -net nic,model=virtio,vlan=0 -net tap,vlan=0,ifname=tap0,script=no,downscript=no -curses
```
```
sudo  qemu-system-x86_64 -hda testing-images.img -net nic -net tap,ifname=tap0,script=no,downscript=no
```
