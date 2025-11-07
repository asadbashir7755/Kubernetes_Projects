volumes in k8s

all concepts related to that

we use it to secure data if pod deleted

to presist data in case pod delete 
bind(presist) data with host 

how? do this?
need>>>
presistent volume

presistent volume claim

A PersistentVolume (PV) is a real piece of storage (e.g., a directory, a disk, or a cloud volume) that Kubernetes manages and makes available to Pods.

Think of it as a physical drive in Kubernetes world.
You create it once, and it stays available for use by any Pod via a claim.