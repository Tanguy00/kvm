# https://libvirt.org/formatnetwork.html

**C'est ce qui nous permet de gérer la mise en réseau des machines virtuelles.**
NAT : isole les VM par groupe, elles peuvent communiquer entre-elles dans ce groupe, ainsi que faire du NAT avec l'hôte (= NAT network).
Route : isole les VM par groupe, elles peuvent communiquer entre-elles dans ce groupe (= LAN segment).
Open : les machines virtuelles ne sont pas isolés, elles sont vues et peuvent voir sur le réseau de l'hôte (= Bridge).
No forward : les machines sont isolées complètement (= Host only).

## EXEMPLE DE CONFIGURATION
Simple réseau avec un DHCP, des DNS spécifiques + un exemple de réservation d'adresse IP et un exemple de mapping IP/NDD.

```xml
<network>
  <name>lab_nat</name>
  <forward mode='nat'/>
  <bridge name='virbr1'/>  # Interface locale pour libvirt (voir $(ip -c a))

  <dns>
    <forwarder addr="1.1.1.1"/>  # Adresse d'un des DNS pour ce network
    <forwarder addr="9.9.9.9"/>  # Idem
    <host ip='13.14.15.16'>                     # Pour définir un enregistrement DNS spécifique
      <hostname>tanguynicolas.fr</hostname>     #
      <hostname>www.tanguynicolas.fr</hostname> #
    </host>                                     #
  </dns>

  <ip address='10.0.0.1' netmask='255.255.255.0'>  # Addresse de la gateway, du dns, du dhcp, ...
    <dhcp>
      <range start='10.0.0.101' end='10.0.0.110'/>                               # Plage d'adresses
      <host mac="00:16:3e:77:e2:ed" name="foo.example.com" ip="192.168.122.10">  # Pour définir une réservation d'addresse
    </dhcp>
  </ip>
</network>
```

