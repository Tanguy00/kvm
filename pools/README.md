[Documentation](https://libvirt.org/storage.html)<br />

**C'est l'endroit où vont être entreposés les données (disques) d'une machine virtuelle.**
- Directory : dans un répertoire de l'hôte.
- Filesystem : dans un FS (dédié ?) de l'hôte.
- Network filesystem : dans un FS (dédié ?) distant.
- Logical volume : dans un LV (dédié ?) de l'ĥote.
- Disk : dans un disque (dédié ?) de l'hôte.
- SCSI : ...
- iSCSI : ...
- ZFS : ...
- Vstorage : ...
...

## EXEMPLE DE CONFIGURATION
Simple pool de type directory, sur l'hôte.

```xml
<pool type="dir">
  <name>lab_directory</name>  # Nom du pool
  <target>
    <path>/var/lib/virt/images</path>  # Emplacement pour les données (ici celui par défaut)
  </target>
</pool>
```

