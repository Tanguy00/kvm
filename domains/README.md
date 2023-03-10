[Documentation](https://libvirt.org/formatdomain.html)<br />

**C'est la définition de nos machines virtuelles.**<br /><br />
Pour crer une VM en CLI, deux possibilités :
- `virt-install` : toutes les options dans la ligne de commande.
- `virsh { define | create }` : toutes les options dans un fichier XML.
<br />

## EXEMPLE DE CONFIGURATION avec virt-install
Simple machine virtuelle, pour un lab, en utilisant un pool et un network précédement créé.

```bash
virt-install \
    --name debian_lab \
    --description "Machine virtuelle de test" \
    --memory unit=MiB,memory=2048 \
    --vcpus 2 \
    --disk path="/home/tn/KVM/images/debian_lab.qcow2",size=20
    --network network=lab_nat
    --cdrom "/home/tn/KVM/iso/"
```
<br />

## EXEMPLE DE CONFIGURATION avec virsh
Simple machine virtuelle, pour un lab, en utilisant un pool et un network précédement créé.

```xml
<domain type='kvm'>
  <name>debian11</name>
  <memory unit='MiB'>2048</memory>
  <vcpu>2</vcpu>
  <os>
    <type arch='x86_64'>hvm</type>
    <boot dev='cdrom'/>                # Sur quoi la VM va démarrer : hd, cdrom, ...
  </os>
  <cpu mode='host-passthrough' check='none' migratable='on'/>
  <clock sync="localtime"/>

  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>

  <devices>
    <emulator>/usr/bin/qemu-system-x86_64</emulator>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/home/tn/KVM/images/debian_lab.qcow2>
      <target dev='vda' bus='virtio'/>
    </disk>
    <disk type='file' device='cdrom'>
      <driver name='qemu' type='raw'/>
      <source file='/home/tn/Downloads/debian-11.5.0-amd64-netinst.iso'/>
      <target dev='vda' bus='virtio'/>
      <readonly/>
    </disk>
    <interface type='network'>
      <source network='lab_nat'/>
      <model type='virtio'/>
    </interface>
    <serial type='pty'>
      <target type='isa-serial' port='0'>
        <model name='isa-serial'/>
      </target>
    </serial>
    <console type='pty'>
      <target type='serial' port='0'/>
    </console>
  </devices>
</domain>
```
<br />
