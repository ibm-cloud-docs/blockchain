---

copyright:
  years: 2014, 2023
lastupdated: "2023-02-17"

keywords: network components, IBM Cloud Kubernetes Service, backup, restore, disaster, peer, orderer, ordering node, LevelDB, CouchDB

subcollection: blockchain

---



{{site.data.keyword.attribute-definition-list}}



# Backing up and restoring components and networks
{: #backup-restore}

Users might want to back up their components individually or the network generally for a number of reasons. While it's a best practice to back up components before upgrading them to a new Fabric version, network backups can be useful for cases in which a flawed smart contract causes invalid transactions to be written to the ledger. While a proper development and testing cycle for a smart contract catches these errors, it might be necessary to reinstate a previous version of the network before the invalid transactions were written.

**Taking snapshots as described in this topic does not protect against the deletion of a component or entire environment by a user**. This is because restoring a deleted component or environment requires more information than is stored in a snapshot. Also, when a component or environment is deleted, the original volumes (which contain things like certificates) and all snapshot data are deleted.
{: important}

##  Overview
{: #backup-restore-overview}

As with anything that is deployed to a Kubernetes-based cluster, backups of components and networks in the {{site.data.keyword.blockchainfull}} Platform are a matter of backing up its [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}. These volumes are where ledgers and other types of storage are mounted so they can be used by nodes.

As a result, "backing up" a component or a network is the process of saving a copy of the relevant persistent volumes, while "restoring" a component or network involves bringing up components and pointing them to these saved volumes. The number of volumes that are associated with a node depends on the type of node. Peers have either one persistent volume (if using LevelDB) or two persistent volumes (if using CouchDB), while each node in the ordering service has a single persistent volume.

Because an ordering node cannot pull blocks from a peer, **all peers must be backed up before the ordering nodes**. This order ensures that the peers do not have any blocks on their ledgers that don't appear on the ledgers of the backed-up ordering nodes. If any peers have blocks that the ordering nodes do not have, the ledgers cannot be synchronized, making the restoration of the network impossible. If you are backing up your ledger as part of an upgrade to a new Fabric version, you do not have to synchronize this backup with the rest of the network. However, make sure to keep at least one ledger backup that is synchronized with the rest of the network in case a larger network restoration is necessary.
{: important}

If you're using CouchDB, which is mounted in a separate container in the peer pod, back up the persistent volume associated with CouchDB before the volume associated with the peer pod. This is because the peer checks on CouchDB after booting and if anything is missing from CouchDB, the peer can push transactions to it which CouchDB integrates into the database. However, any transactions in the state database cannot be pushed back to the peer.

As a rule, it is a good practice to schedule backups (also known as "snapshots") to happen at the same time across the network. For example, if all of the peers in the network are using CouchDB, schedule all of the CouchDB persistent volumes to be backed up at the same time, followed by all of the peer persistent volumes (again, at the same time), and then the ordering node persistent volumes (if multiple organizations are contributing ordering nodes, these snapshots must be coordinated with other organizations). For an example of a backup schedule a network might choose to adopt, check out [Scheduling snapshots](#backup-restore-schedule-snapshot).

While it is a best practice to periodically back up your Certificate Authority, these backups do not have to be coordinated with the rest of the network. CAs with a local database have a single persistent volume. If you are using `PostgreSQL`, the CA has an additional persistent volume associated with it that must also be backed up. For information about how to take backups of a `PostgreSQL` database, check out [Managing Backups](/docs/databases-for-postgresql?topic=cloud-databases-dashboard-backups){: external} from the Databases for PostgreSQL documentation.

The following node-specific guidance is provided to help plan your disaster recovery strategy.

### Backup considerations for each node type
{: #backup-restore-node-considerations}

Consult the following chart to help you plan your strategy for taking backups:

|  Backup  | Restore |
|:---------|---------|
| CAs must be stopped and backed up following the instructions that are provided in this topic. | You can restore a CA without impacting the peers or the ordering nodes ability to process transactions. |
{: caption="Table 1. Considerations for node backup and restore - CA" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Certificate Authority (CA)"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Backup  | Transaction processing | Private data considerations |
|:---------|:-----------------------|:----------------------------|
| Ideally, all peers in the network should be backed up daily at the same time. If the peers have a CouchDB state database, CouchDB must be backed up before the peer pod. Ordering nodes should be backed up after the peers. If peers have data that the ordering service does not, synchronization of components cannot occur. | Unless the peers are scaled down to zero as part of backing up private data, transaction processing can continue. | If the peers include private data collections, the peer and ordering nodes must be using Fabric version v.2.2.1 or higher, otherwise every node must be backed up at exactly the same ledger height. This can be achieved by scaling down the resources of the peers to zero as part of the backup. For more information, see [Scheduling snapshots](#backup-restore-schedule-snapshot). |
{: caption="Table 2. Considerations for node back up and restore - Peer" caption-side="top"}
{: #simpletabtable2}
{: tab-title="Peers"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Backup  | Transaction processing | Restore |
|:---------|:-----------------------|:--------|
| Ideally, all nodes in the ordering service should be backed up daily at the same time. The ordering service should be backed up after the peers to ensure that the restoration of components can occur later. | Transaction processing continues as normal during a backup. If a restoration is necessary, the ordering service must be brought down. | **If you are not using private data collections**, you can provision _new_ peers and rejoin the channels. The peers can sync up with the latest data from the ordering service, then you can reinstall any smart contracts. <br>Or, you can restore the peers from a backup older than the ordering nodes. In this case, the peers only must catch up from the last backup, which is faster. Any smart contracts installed since the last backup must be reinstalled. <br><br> **If you are using private data collections**, the peers must be restored to exactly the same block height as the orderers. The best way to ensure this is to back up all peers while the ordering service is also down for the backup. |
{: caption="Table 3. Considerations for node back up and restore - Ordering service" caption-side="top"}
{: #simpletabtable3}
{: tab-title="Ordering service"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}



### Scheduling snapshots
{: #backup-restore-schedule-snapshot}

It is a best practice to create a regular schedule for the snapshotting of your components. If possible, try to coordinate these snapshots with the other members of your network to ensure that the most recent snapshots can be used if necessary. Because no peer snapshots can be used that are more recent than ordering service snapshots, you are bound by your snapshots of the ordering service when attempting to restore.

Even though snapshots do not disrupt the functionality of your nodes, try to schedule snapshots during a time when the fewest transactions are being made. This ensures that differences in ledger heights between peers are kept to a minimum. In widely distributed networks, scheduling such a time might be difficult. Prioritize the proper ordering of snapshots over relative lulls in network activity.
{: tip}

For this example, we assume one backup every 24 hours and retain seven backups.

For the peer pods:
- CouchDB snapshot daily at 3:00 a.m. 
- Peer snapshot daily at 3:05 a.m.

For the ordering node pods:
- Ordering node snapshots daily at 5:00 a.m. The two-hour gap allows sufficient time for all of the peers to be snapshotted from every organization before ordering nodes are snapshotted.

If you are using **private data collections** in your applications, your nodes must be running Hyperledger Fabric 2.2.1 or higher in order to back up peer and ordering nodes while they are running. When restoring from a backup, the peers must be restored from a lower block height than the ordering nodes. While peers can catch up to the block height of the ordering service, the ordering service does not have the actual private data (in a private data transaction, only hashes of the data are committed to the public ledger), rendering the data for these blocks unavailable. Fabric v2.2.1 and higher has a configurable option to lower the priority for the reconciliation of missing private data. Earlier versions constantly attempt to reconcile the missing private data, potentially blocking out reconciliation of other private data. On earlier versions of Hyperledger Fabric, you must back up ordering nodes and peers at exactly the same block heights to avoid this issue. This can be accomplished by scaling the both the peer and ordering node pods to 0 before taking the backup.

## Taking snapshots
{: #backup-restore-take-snapshot}

The overall steps for the backup and restore process apply in all cases and are independent of where your components are deployed. However, the specific commands that are used in this example for creating, listing, and restoring snapshots apply only if you are running on {{site.data.keyword.cloud_notm}}. For other cloud providers or when running in your own datacenter, follow the specific instructions for underlying snapshot creation and management found in the documentation of those providers.

When a snapshot is being taken the node continues to function as normal.
{: tip}

### Node snapshots
{: #backup-restore-peer-snapshot}

Because the process for taking a snapshot is the same for all nodes, only one set of instructions is shown here.
{: tip}

After your node is provisioned, you can get the persistent volumes that are associated with it and the namespace the component is deployed into by issuing:


```
kubectl get pvc --all-namespaces
```
{: codeblock}



Which produces a list of nodes and their persistent volumes similar to this example:

```
NA.M.ESPACE   NA.M.E                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE
n3392fd     orderer1node1-pvc   Bound    pvc-34723fe1-c8e9-4fe7-bdf8-75b8a2f22912   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer2node1-pvc   Bound    pvc-11b34473-bfdf-4fac-962b-5999bb961a50   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer3node1-pvc   Bound    pvc-33ed8cce-21ec-44a8-8a0f-190052b985f9   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer4node1-pvc   Bound    pvc-5fd2b807-2361-406b-802a-a18dbaabe23e   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer5node1-pvc   Bound    pvc-01da603e-3d2b-4b77-a0ac-af3e70e2d2f5   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     ordererorgca-pvc    Bound    pvc-e582ef7f-04ae-4493-9bdb-426c784886c3   20Gi       RWO            ibmc-file-bronze   49d
n3392fd     peera-pvc           Bound    pvc-def5a60c-25f3-4de0-b9cc-2cf8dcf88cc2   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     peera-statedb-pvc   Bound    pvc-494ba122-3501-4413-b719-cbb4b3a8f91b   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     peerc-pvc           Bound    pvc-ba9cee42-4d35-48d9-ad05-85afc7256633   100Gi      RWO            ibmc-file-bronze   48d
n3392fd     peerc-statedb-pvc   Bound    pvc-e945df85-5a32-4d37-991d-b2be334bc09b   100Gi      RWO            ibmc-file-bronze   48d
n3392fd     peerorg1ca-pvc      Bound    pvc-152be78e-202d-43a4-bcf7-81a6a9013c2d   20Gi       RWO            ibmc-file-bronze   49d
```
{: codeblock}

To get the details on a particular volume, issue:

```
kubectl describe pv <VOLUME>
```
{: codeblock}

If you are using the CLI to manage snapshots, you'll need the `volumeId` value. If you are using the cluster UI, you'll need the `Username` value. For an example volume named `pvc-494ba122-3501-4413-b719-cbb4b3a8f91b`, you would issue this command:

```
kubectl describe pv pvc-494ba122-3501-4413-b719-cbb4b3a8f91b
```
{: codeblock}

Which produces a list of details similar to this example:

```
Name:            pvc-494ba122-3501-4413-b719-cbb4b3a8f91b
Labels:         CapacityGb=100
                Datacenter=dal10
                Iops=2
                StorageType=ENDURANCE
                Username=IBM02SEV2046428_106
                billingType=hourly
                failure-domain.beta.kubernetes.io/region=us-south
                failure-domain.beta.kubernetes.io/zone=dal10
                path=IBM02SEV2046428_106data01
                server=fsf-dal1002h-fz.adn.networklayer.com
                volumeId=155617812
Annotations:    ibmFileProvisionerIdentity: 44cc1a5f-b755-11ea-bfae-3692beefed97
                pv.kubernetes.io/provisioned-by: ibm.io/ibmc-file
Finalizers:     [kubernetes.io/pv-protection]
StorageClass:   ibmc-file-bronze
Status:         Bound
Claim:          n3392fd/peera-statedb-pvc
Reclaim Policy: Delete
Access Modes:   RWO
VolumeMode:     Filesystem
Capacity:       100Gi
Node Affinity:  <none>
Message:
Source:
    Type:       NFS (an NFS mount that lasts the lifetime of a pod)
    Server:     fsf-dal1002h-fz.adn.networklayer.com
    Path:       /IBM02SEV2046428_106/data01
    ReadOnly:   false
Events:         <none>
```
{: codeblock}

You can then order snapshots for that volume, which can be done by using either the CLI or the cluster UI. For information on how to take the snapshot, see [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots){: external}.

In this case, "ordering" refers to the "order to take snapshots" and not "ordering nodes".
{: tip}

For our example, we use the CLI and order a single snapshot by using 5 GB of space by issuing:

```
ibmcloud sl file snapshot-order 155617812 -s 5
```
{: codeblock}

You might need more or less than 5 GB of space, depending on the size of the pod and the resources that have been used on it. The amount of space that is required depends on the size of your transactions, the quantity of transactions, and how many snapshots (or backups) that you want to keep. To determine how much storage you are using, [monitor](/docs/blockchain?topic=blockchain-ibp-monitoring) the disk space that is consumed by your nodes so that you know when to add storage. You can use this data to inform how much storage is being used, and how fast it is growing. Also, if you monitor the size of each snapshot, you can use the snapshot size along with the number of snapshots you want to keep to determine when more snapshot space is required.
{: tip}

Taking a snapshot incurs charges on your account. Press `y` to accept the charges and order the snapshot.

Alternatively, you can enable snapshots to be taken on a regular schedule for a particular volume. For information on how to set of a snapshot schedule, see
[Adding a Snapshot schedule](/docs/FileStorage?topic=FileStorage-managingSnapshots#addscheduleUI){: external}.

Again, for our example, we use the CLI to set up a daily snapshot of a CouchDB volume at 3:00 a.m. and retain seven snapshots.

```
ibmcloud sl file snapshot-enable 155617812 -s DAILY -c 7 -r 3
```
{: codeblock}

Which produces a message similar to the following:

```
OK
DAILY snapshots have been enabled for volume 155617812.
```
{: codeblock}

We can now list the snapshot schedule to verify it's in place by issuing:

```
ibmcloud sl file snapshot-schedule-list 155617812
```
{: codeblock}

Which produces a message similar to the following:

```
id       active   type    replication   date_created           minute   hour   day   week   day_of_week   date_of_month   month_of_year   maximum_snapshots   
542130   *        DAILY   -             2020-09-02T16:35:58Z   0        3      *     -      *             *               *               7   
```
{: codeblock}

Repeat the following steps for each pod you want to back up, remembering to schedule your backups in the proper order.

## Restoring nodes
{: #backup-restore-restore}

If you have to restore a node, first cancel any scheduled snapshots. Only restore from a snapshot that does not have bad or corrupted data. After a node has been restored, you can reimplement your snapshot schedule.
{: tip}

Here we have two examples. In the first example, we show the steps for restoring peer nodes, while in the second example, we describe the steps for restoring the ordering service, which requires restoring peer nodes as well.

Because the details for finding the persistent volumes that are associated with each ordering node and restoring snapshots are identical to what is documented for the peer, the commands are only listed once, in [the peer instructions](#backup-restore-restore-peer). However, additional considerations when restoring ordering nodes are documented in [Restoring an ordering node](#backup-restore-restore-orderer).

### Sequencing restorations
{: #backup-restore-restore-sequence}

Just as when backing up, care must be taken to restore things in the proper order. When restoring ordering nodes, for example, even if you only restore one node, scale down all of the pods of the ordering nodes by reducing their resources to zero. If every ordering node must be restored, the best practice is to stop them all at once and then restore them all at once. Then, make sure all of them come up and give them a few minutes to elect a leader and establish quorum before driving any transactions.

If you are restoring the ordering service from a backup, you must also either create new peers or restore all peers to a backup older than the ordering service. Peers restored from an older backup can catch up to the latest data by receiving blocks from the ordering service.

When restoring a peer, make sure to restore backups of CouchDB older than the backup of the peer pod. If your state database has transactions your peer does not have, the database cannot sync with the peer.

### Restoring a peer
{: #backup-restore-restore-peer}

To restore a peer, you need to:

1. Find the deployment that you want to restore.
2. Scale it down to zero to stop the node.
3. Restore the node from the backup.

First, get the list of deployments:

```
kubectl get deployments
```
{: codeblock}

As before, this produces a list of nodes and their persistent volumes similar to this example:

```
NA.M.E            READY   UP-TO-DATE   AVAILABLE   AGE
ibp-operator    1/1     1            1           48d
orderer1node1   1/1     1            1           20h
orderer2node1   1/1     1            1           22h
orderer3node1   1/1     1            1           22h
orderer4node1   1/1     1            1           22h
orderer5node1   1/1     1            1           22h
ordererorgca    1/1     1            1           48d
peera           1/1     1            1           20h
peerc           1/1     1            1           20h
peerorg1ca      1/1     1            1           48d
```
{: codeblock}

Then scale down the node by setting the `replicas` value to `0`.


```
kubectl patch <component> <componentName> --type merge --patch '{"spec":{"replicas":0}}' -n <namespace>
```
{: codeblock}



Where `<component>` is one of: `ibppeer`, `ibporderer`,  or `ibpca`.

In this example, scale down `peera`:

```
kubectl patch <ibppeer> peera --type merge --patch '{"spec":{"replicas":0}}' -n n3392fd
```
{: codeblock}

Then look at your deployments again to make sure that the peer is scaled down:

```
kubectl get deployments
```
{: codeblock}

You'll receive a response similar to:

```
NA.M.E            READY   UP-TO-DATE   AVAILABLE   AGE
ibp-operator    1/1     1            1           48d
orderer1node1   1/1     1            1           20h
orderer2node1   1/1     1            1           22h
orderer3node1   1/1     1            1           22h
orderer4node1   1/1     1            1           22h
orderer5node1   1/1     1            1           22h
ordererorgca    1/1     1            1           48d
peera           0/0     0            0           20h
peerc           1/1     1            1           20h
peerorg1ca      1/1     1            1           48d
```
{: codeblock}

Note that `peera` is not ready.

Use the commands in the [Peer snapshot](#backup-restore-peer-snapshot) section to find the persistent volumes that are associated with the peer. Peers using CouchDB have two volumes.

For each volume, list the available snapshots for that volume. For information on how to list available snapshots, see [Listing all Snapshots with Space Used Information and Management functions](/docs/FileStorage?topic=FileStorage-managingSnapshots#listsnapshotUI){: external}.

For our example, we use CLI and restore a specific snapshot from our volume ID for the CouchDB pod that is associated with `peera`:

```
ibmcloud sl file snapshot-list 155617812
```
{: codeblock}

Which shows the available snapshots:

```
id          user_name             created                     size_bytes   notes   
167189176   IBM02SEV2046428_106   2020-09-03T03:00:00-05:00   143089      
```
{: codeblock}

Pick the snapshot you'd like to restore from the list, keeping in mind that the CouchDB backup must be older than the peer pod backup, and restore it. For information on how to restore the snapshot, see [Restoring storage volume to a specific point-in-time by using a snapshot](/docs/FileStorage?topic=FileStorage-managingSnapshots#restorefromsnapshotUI){: external}.

Restore by using the snapshot:

```
ibmcloud sl file snapshot-restore <volumeid> <snapshotid>
```
{: codeblock}

There is only one snapshot available here, so select it:

```
ibmcloud sl file snapshot-restore 155617812 167189176
```
{: codeblock}

A successful snapshot restoration looks similar to:

```
OK
File volume 155617812 is being restored using snapshot 167189176.
```
{: codeblock}

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}

Repeat the process for the other volume used by the peer container, which can be found by using the same commands. Choose the 3:05 a.m. snapshot corresponding to the 3:00 a.m. snapshot you restored for CouchDB volume.

Once both snapshots are restored, scale the deployment back up to one to redeploy the pod with the new data. First, check the status of the pods:

```
kubectl get deployments
```
{: codeblock}

You'll receive a response similar to:

```
NA.M.E            READY   UP-TO-DATE   AVAILABLE   AGE
ibp-operator    1/1     1            1           48d
orderer1node1   1/1     1            1           20h
orderer2node1   1/1     1            1           22h
orderer3node1   1/1     1            1           22h
orderer4node1   1/1     1            1           22h
orderer5node1   1/1     1            1           22h
ordererorgca    1/1     1            1           48d
peera           0/0     0            0           20h
peerc           1/1     1            1           20h
peerorg1ca      1/1     1            1           48d
```
{: codeblock}

Then scale the `peera` pod back up by setting the replicas value to `1`.


```
kubectl patch <component> <componentName> --type merge --patch '{"spec":{"replicas":1}}' -n <namespace>
```
{: codeblock}



Where `<component>` is, once again, one of: `ibppeer`, `ibporderer`,  or `ibpca`.

In this example, scale up `peera`:

```
kubectl patch <ibppeer> peera --type merge --patch '{"spec":{"replicas":1}}' -n n3392fd
```
{: codeblock}

Once the pod is back up, the peer is now restored, with the same peer channel membership, installed smart contracts, and ledger height as when the snapshot was taken. Because the peer immediately starts pulling new blocks from the ordering service or other peers once it comes up, the ledger height might increase.

### Restoring an ordering node
{: #backup-restore-restore-orderer}

The process for restoring an ordering node is largely similar to the process for restoring a peer. However, there are a few additional things to keep in mind.

1. You must stop **all** the ordering nodes in the cluster by scaling down each deployment to zero before restoring **any** of the ordering nodes. Note after the nodes have been scaled down, it is not possible to transact. Client applications must resubmit their transaction requests after the ordering service is restored.
2. If you restore all ordering nodes, you must scale all the deployments up to `1` to restart them and give them five minutes to determine leadership before accepting any traffic. It is a best practice to also stop all peers on the network during this process.
3. You must then restore **all** peers that are connected to that ordering service to a backup taken earlier than the ordering service backup you are restoring to, or provision new peers.

In our example, the flow you would take is:

1. Scale down all five ordering nodes and peer deployments to zero to stop all traffic on the network.
2. Restore the peer nodes to the 3:00 a.m. CouchDB snapshot and the 3:05 a.m. peer pod snapshot.
3. Restore each ordering node to its 5:00 a.m. snapshot taken the same day as the peer snapshots you restored.
4. Scale all five ordering service deployments up to `1` and wait for five minutes after they have started to sync up.
5. Scale all peer deployments up to `1`.
