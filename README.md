---
title: "Comment remplacer rapidement le disque système d'un ordinateur portable ou de bureau par un plus grand à l'aide de Proxmox ?"
categories:
  - Blog
tags:
  - clone disk system
  - migration du système
  - Proxmox
---

#  Comment remplacer rapidement le disque système d'un ordinateur portable ou de bureau par un plus grand à l'aide de Proxmox ?

## Prerequisites

### Hardware Requirements
- 2 disks NVME : l'ancien avec le système et le nouveau plus grand.
- Un ordinateur avec un système Proxmox où il est possible d'installer physiquement deux disks nvme. (Par exemple, Proxmox peut être installé sur une clé USB).
- Un ordinateur avec la capacité d'exécuter un navigateur et connecté via un réseau à un ordinateur avec Proxmox.

**Pour préparer un nouveau disque plus grand avec un système provenant d'un ancien disque plus petit, vous devez procéder comme suit :**

### 1. Prenez un disque de l'ordinateur que nous mettons à niveau (petit, ancien).
### 2. Installez les anciens et les nouveaux disques dans le système Proxmox.
### 3. Créez une machine virtuelle Linux qui démarrera à partir de l'iso ubuntu-24.04.1-desktop-amd64.iso avec les disques correspondants montés.

**Le fichier de configuration pourrait ressembler à ceci :**
```
nano /etc/pve/qemu-server/111.conf
```
**Exemple de fichier :**
```
balloon: 0
boot: order=ide2
cores: 2
cpu: x86-64-v2-AES
hotplug: disk
ide2: local:iso/ubuntu-24.04.1-desktop-amd64.iso,media=cdrom,size=6057964K
memory: 5000
meta: creation-qemu=9.0.2,ctime=1735929554
name: vsanliveCD
net0: virtio=BC:24:11:61:08:45,bridge=vmbr0
net1: virtio=BC:24:11:19:7D:0B,bridge=vmbr1,link_down=1,mtu=1
net2: virtio=BC:24:11:62:99:05,bridge=vmbr2,link_down=1,mtu=1
numa: 0
ostype: l26
sata0: /dev/nvme0n1
sata1: /dev/nvme3n1
scsihw: virtio-scsi-single
smbios1: uuid=0e7e4145-efcb-404b-b609-388cf4402047
sockets: 1
vmgenid: f71ed752-1d0b-4337-87af-4a51a9d96c61

```
### 4. Migration du système de l'ancien vers le nouveau disque
**Nous démarrons la machine virtuelle Ubuntu Linux dans le navigateur d'un autre ordinateur, en nous connectant au système avec Proxmox. Nous lançons le programme GParted et, en utilisant l'interface graphique, initialisons uniquement le nouveau disque comme l'ancien (généralement gpt) et copions les partitions du disque de l'ancien vers le nouveau une par une, en augmentant la taille des partitions selon les besoins. Nous accomplissons nos actions. Le disque système a été cloné. Nous quittons Linux Ubuntu. Nous arrêtons le système et éteignons l'ordinateur avec le système Proxmox.**
### 5. Nous mettons le nouveau disque dans l'ordinateur dont le disque a été mis à niveau et l'allumons !


#ITEnterprise #ContinuitéActivité #InfrastructureSécurisée
