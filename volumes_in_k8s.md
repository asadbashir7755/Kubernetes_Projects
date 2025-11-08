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



so the flow is that when we create pv and pvc after that when we create pod inside pod there must be containeer wen k8 s will schedule pod it will check pod have container which have  volume which is claiming from pvc then it will check on which node pv is assigned then it will schdule pod on that node also right . and replicas =5 only one pod can access  pv just bcz of ReadWriteOnce access mode 