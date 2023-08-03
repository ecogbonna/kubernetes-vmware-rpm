# Compute Resources

Note: You must have VMware configured at this point



- Node IP addresses ranges 192.168.1.0/24

    | VM            |  VM Name               | Purpose       | IP            | CPU    | RAM  |
    | ------------  | ---------------------- |:-------------:| :-------------|:------:|-----:|
    | master        | master.localdomain     | Master        | 192.168.1.70  |     2  | 4096 |
    | node1         | node1.localdomain      | Worker        | 192.168.1.71  |     2  | 4096 |
    | node2         | kubernetes-ha-worker-1 | Worker        | 192.168.1.72  |     2  | 4096 |

    > These are the default settings. They can be changed.


Prev: [Prerequisites](01-prerequisites.md)<br>
Next: [Installing kubeadm](03-installing-kubeadm.md)