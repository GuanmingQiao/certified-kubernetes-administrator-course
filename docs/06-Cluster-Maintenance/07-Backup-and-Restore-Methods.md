# Backup and Restore Methods
  - Take me to [Video Tutorial](https://kodekloud.com/topic/backup-and-restore-methods/)
  
In this section, we will take a look at backup and restore methods

## Backup Candidates
 
 ![bc](../../images/bc.PNG)
 
## Resource Configuration
- Imperative way
  
  ![rci](../../images/rci.PNG)

- Declarative Way (Preferred approach)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: myapp-pod
    labels:
      app: myapp
      type: front-end
  spec:
    containers:
    - name: nginx-container
      image: nginx
  ```
 ![rcd](../../images/rcd.PNG)
 
- A good practice is to store resource configurations on source code repositories like github.

  ![rcd1](../../images/rcd1.PNG)

## Backup - Resource Configs

  ```
  $ kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml (only for few resource groups)
  ```

- There are many other resource groups that must be considered. There are tools like **`ARK`** or now called **`Velero`** by Heptio that can do this for you.

  ![brc](../../images/brc.PNG)
  
## Backup - ETCD
- So, instead of backing up resources as before, you may choose to backup the ETCD cluster itself. 
  
  ![be](../../images/be.PNG)
  
- You can take a snapshot of the etcd database by using **`etcdctl`** utility snapshot save command.
  ```
  $ ETCDCTL_API=3 etcdctl snapshot save snapshot.db
  ```
  ```
  $  ETCDCTL_API=3 etcdctl snapshot status snapshot.db
  ```
  ![be1](../../images/be1.PNG)
  
## Restore - ETCD Running As Pod (Stacked ETCD)
- `ETCDCTL_API=3 etcdctl --data-dir /var/lib/etcd-from-backup snapshot restore snapshot.db` Restore ETCD from snapshot.db file, and write a new data manifest to /var/lib/etcd-from-backup
- Update `/etc/kubernetes/manifests/etcd.yaml` to use the new data volume. Still mount it to etcd-data directory in the container.
  ```
  volumes:
  - hostPath:
      path: /var/lib/etcd-from-backup
      type: DirectoryOrCreate
    name: etcd-data
  ```
- After this file is modified, all components on master node will restart. `kubectl` will not be accessible for a few minutes. Can use `watch "crictl ps | grep etcd"` to mointor the restart status.

## Restore - ETCD Running As Daemon (Remote ETCD)
- Assuming you have a backup file locally in a controlplane node or maintainer node. Use `scp /opt/cluster2.db etcd-server:/root/cluster2.db` to copy the backup file into remote etcd server.
- Ssh into the ETCD server. Run `ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/pki/ca.pem --cert=/etc/etcd/pki/etcd.pem --key=/etc/etcd/pki/etcd-key.pem --data-dir /var/lib/etcd-data-new snapshot restore /root/cluster2.db`
- ETCD is running as a systemd service (or daemon) in the server. Similar to how we modify the manifest yaml file for a Pod, we modify the `systemd` configuration for a daemon.
  - `vim /etc/systemd/system/etcd.service`
  - Change `--data-dir=/var/lib/etcd-data` to `--data-dir=/var/lib/etcd-data-new`, and save the change.
  - Make sure the read permission on this file is not changed: `chown -R etcd:etcd /var/lib/etcd-data-new`
- Let systemd know about the configuration change: `systemctl daemon-reload`
- Restart the changed service `systemctl restart etcd`

#### With all etcdctl commands specify the cert,key,cacert and endpoint for authentication.
```
$ ETCDCTL_API=3 etcdctl \
  snapshot save /tmp/snapshot.db \
  --endpoints=https://[127.0.0.1]:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/etcd-server.crt \
  --key=/etc/kubernetes/pki/etcd/etcd-server.key
```

  ![erest](../../images/erest.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/


 
