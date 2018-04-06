# Ansible-Cuckoo-installation
Dieses Repo ermöglicht die automatische Installation und Konfiguration mit Ansible. Die aktuelle Konfiguration ermöglicht nur
die Installation auf dem aktiven Host und die Nutzung von VirtualBox.

Die Vorbereitung und Installation der Analysemaschinen kann nach dem Ansible-Durchlauf manuell mit vmcloak durchgeführt werden. Die Konfiguration der Analyse-Nodes muss in der Variablendatei erfolgen. Alternativ werden die Analysemaschinen manuell nach der offiziellen [Anleitung](https://cuckoo.sh/docs/installation/guest/index.html) erstellt und konfiguriert.

## Installation

Dieses Repo klonen oder herunterladen und entpacken.

Weiterhin muss zur Vorbereitung Ansible und Python installiert werden:
```bash
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```

Eine vollständige Installation von cuckoo und allen notwendigen Voraussetzungen kann über folgenden Befehl ausgeführt werden. Vorher sollten die relevanten Variablen angepasst werden. Dazu wird die Datei (inventory/host_vars/cuckoo_host) bearbeitet (siehe Kapitel Anpassung).
```bash
$ ansible-playbook -i inventory/hosts sity.yml -kK
```

## Nutzung

Nun können Cuckoo-Rooter, Cuckoo und Cuckoo-Web gestartet werden (aus dem WorkingDirectory heraus):
```bash
$ cuckoo rooter --sudo
$ cuckoo
$ cuckoo web
```

## Anpassung

Die Variablen in inventory/hosts_vars/cuckoo_host müssen vor dem Start des Playbooks angepasst werden.

TODO: Beschreibung Variablen

## Test

Um das Playbook zu testen wurde eine Vagrantbox und ein Dummy-Inventory erstellt. Der Test kann folgendermaßen ausgeführt werden (Nutzer/Passwort: `vagrant`):
``` bash
$ cd tests/cuckoo_host/
$ vagrant up
$ cd ../
$ ansible-playbook -i inventory tests.yml -kK
```

## Bekannte Fehler

Das virtuelle Interface muss nach einem Neustart des Hosts unter Umständen neu konfiguriert werden. Dies kann entweder über Ansible
```bash
$ ansible-playbook -i inventory/hosts sity.yml -kK --tags "role::cuckoo::configure"
```
oder manuell erledigt werden:
```bash
$ VBoxManage hostonlyif Create
$ VBoxManage hostonlyif ipconfig <hostonlyadapter> --ip <ip>
$ sudo ifconfig <hostonlyadapter> <ip> netmask <netmask> broadcast <broadcastip>
```
