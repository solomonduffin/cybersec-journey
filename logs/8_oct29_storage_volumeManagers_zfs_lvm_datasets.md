## Storage

Okay so I'm struggling with how much detail I should go into here. The problem is I'm only understanding certain things at a surface level and I don't care to necessarily go so much deeper right now. But at a later date I would maybe come back to this section and fill in details, which maybe defeats the purpose of the logs? I'm not sure. I think this is for me and my own understanding and storage of concepts to come back to so that's probably most useful for myself.

### Volume Managers, ZFS, Pools, LVMs
Anyway, I'm trying to understand how ZFS pools, or storage pools work. And then how Datasets are on top of that. And then the question is, is that a Proxmox concept, a ZFS concept, or a general concept? Does it transcend the layer I'm working in and is relevant in many layers of storage?

So I think it does. Basically a pool is a connection of multiple storage drives, which can now be managed together as a single drive. But it's different from say RAID which actually combines the disks logically for redundancy. So pools don't provide redundancy necessarily, they just allow you to "combine" storage into one controllable and mountable unit, essentially it's just virtual storage. The systems that create pools, or logical volumes, are called Volume Managers, and each have their own system and features.

So ZFS is a volume manager (zpool is the name of the volume manager aspect) that can both apply a RAID configuration over a set of physical drives, and then manage the "single" volume. But, it's also a filesystem, which is what makes ZFS unique compared to some other volume managers. It also manages the metadata of the disk, directories, files, permissions, snapshots, etc.. It decides where to put things and how to maintain parity using the RAID scheme. 
So it replaces the common Linux stack of mdadm + LVM + ext4.

The term Pool specifically is the volume managed by ZFS. A logical volume (LV) is the term for the virtual storage managed by LVM. LVM is not a filesystem, it's just a volume manager. It can sit on top of a RAID array, or a normal drive, and does not need to understand how that block of storage functions, it just manages it. Then something like ext4 comes in to organize how files will be stored on the drive, and actively handle directories and navigation and such.

https://en.wikipedia.org/wiki/Logical_volume_management


### Datasets
Datasets are another ZFS specific term. They're like separated directories in the pool. NOT partitions. They can have their own settings, sharing settings, mount points, snapshots, etc.. They share free space from the pool dynamically. They're like sub-filesystems.




<details>
<summary> GPT storage pools concept </summary>
A **storage pool** means:

> “A logical collection of one or more physical storage devices that are managed together as a single resource.”

From this pool, you can allocate *logical units* (volumes, datasets, virtual disks, etc.) for specific purposes.

So conceptually:

```
Physical drives  →  Storage Pool  →  Logical Volumes / Datasets
```

---

### 🧩 Where the term is used

| Platform / Tech         | How “storage pool” is used                                                | Example                                  |
| ----------------------- | ------------------------------------------------------------------------- | ---------------------------------------- |
| **ZFS (Linux / BSD)**   | Called a *zpool* — a group of drives managed together                     | `zpool create tank mirror sda sdb`       |
| **LVM (Linux)**         | Uses *volume groups* instead of “pools,” but same idea                    | `vgcreate mypool /dev/sda /dev/sdb`      |
| **Proxmox VE**          | Uses pools as logical storage backends (ZFS, LVM, NFS, etc.)              | `tank` pool contains VM disks, backups   |
| **Synology NAS / QNAP** | Combines drives into a *storage pool* before making volumes               | “Storage Pool 1” from 3 drives in RAID5  |
| **VMware / Hyper-V**    | Combines physical disks into a *storage pool* for virtual machine storage | “VM Storage Pool” used by multiple hosts |
| **Ceph / SANs**         | Defines pools of replicated data objects across nodes                     | “rbd pool” in Ceph cluster               |

---

### ⚙️ Why it matters

Without the pooling concept, every disk must be managed separately (mount points, partitions, capacity tracking, etc.).
Pooling lets you:

* Expand easily by adding disks
* Share free space dynamically
* Apply redundancy/compression at a higher level
* Simplify provisioning for virtual machines or containers
  
</details>


