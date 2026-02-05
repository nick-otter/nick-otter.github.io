---
layout: post
title:  "Kubernetes ReadWriteOnce Explained"
tags: kubernetes
---

| Muthe Nagavamsi | [Mutha Nagavamsi](https://www.linkedin.com/in/nagavamsi?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAAFslY8Bj46ZzxqLHqETxgkOyeo1Jbn7hX4&lipi=urn%3Ali%3Apage%3Ad_flagship3_detail_base%3B4pe2aardQrWCaKNZ0PPfyA%3D%3D) |

ReadWriteOncePod is a new access mode in Kubernetes. 

It enables data isolation, which is key to maintain Kubernetes integrity.

Find out how, let's go.

In K8s PersistentVolumes has four access modes.

→ **ReadWriteOnce**
→ **ReadOnlyMany**
→ **ReadWriteMany**
→ **ReadWriteOncePod**

These access modes provide desired level of access to the persistent volumes (PVs).

Persistent Volume Claim (PVC's) enables applications in Kubernetes to dynamically utilize persistent volumes.

Creating a Pod with a PVC in ReadWriteOncePod access mode brings joy to K8s.

Because Kubernetes guarantees exclusive PVC access for that specific Pod.

In other words, no other Pod can read from or write to this PVC, even if the original Pod goes to an unknown or failed state.

How can you enable ReadWriteOncePod access mode?

1. Create a PersistentVolumeClaim with spec accessModes set to ReadWriteOncePod.<br>
2. If you need a storage volume that can be reused on a pod failure, then you can use ReadWriteOnce access mode.<br>
3. This will ensure the PVC availability even when the old Pod goes to an unknown or failed state.

![](/assets/k8_1.png)
