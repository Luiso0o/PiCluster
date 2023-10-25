# How to install Kubernetes from scratch using Raspberry Pi and ClusterHAT

<img src="/img/clusterhat.png" width="720">

I've installed from https://clusterctrl.com/


64-bit for Pi 3/3+/4/400/CM3/CM4/Zero2 models:

* [CNAT - Lite Controller](https://dist.8086.net/clusterctrl/bullseye/2023-05-03/2023-05-03-4-bullseye-ClusterCTRL-arm64-lite-CNAT.zip) - Lite Bullseye image for the controller (3/3+/4/400).

* [P1](https://dist.8086.net/clusterctrl/bullseye/2023-05-03/2023-05-03-4-bullseye-ClusterCTRL-arm64-lite-p1.zip) - Lite Bullseye image (Zero 2/A3+/CM3/CM4 only) P1.

* [P2](https://dist.8086.net/clusterctrl/bullseye/2023-05-03/2023-05-03-4-bullseye-ClusterCTRL-arm64-lite-p2.zip) - Lite Bullseye image (Zero 2/A3+/CM3/CM4 only) P2.

* [P3](https://dist.8086.net/clusterctrl/bullseye/2023-05-03/2023-05-03-4-bullseye-ClusterCTRL-arm64-lite-p3.zip) - Lite Bullseye image (Zero 2/A3+/CM3/CM4 only) P3.

* [P4](https://dist.8086.net/clusterctrl/bullseye/2023-05-03/2023-05-03-4-bullseye-ClusterCTRL-arm64-lite-p4.zip) - Lite Bullseye image (Zero 2/A3+/CM3/CM4 only) P4.


# Installing MicroK8s

Follow this section for each of your Pis. Once completed you will have MicroK8s installed and running everywhere.

SSH into your first Pi and there is one thing we need to do before we get cracking. We need to enable c-groups so the kubelet will work out of the box. To do this you need to modify the configuration file /boot/firmware/cmdline.txt:


`sudo nano /boot/cmdline.txt`

And add the following options:

`cgroup_enable=memory cgroup_memory=1`

Now save the file in your editor and reboot:

`sudo reboot`

Now we install snapd

`sudo apt install snapd`

Once that’s done we can now Install the MicroK8s snap:


`sudo snap install microk8s --classic`

Channels are made up of a track (or series) and an expected level of stability, based on MicroK8s releases (Stable, Candidate, Beta, Edge). For more information about which releases are available, run

`snap info microk8s`

## Discovering MicroK8s
Before going further here is a quick intro to the MicroK8s command line:

The start command will start all enabled Kubernetes services: `microk8s.start`

The inspect command will give you the status of services: `microk8s.inspect`

The stop command will stop all Kubernetes services: `microk8s.stop`

You can easily enable Kubernetes add-ons, eg. to enable “kubedns”: `microk8s.enable dns`

To get the status of the cluster: `microk8s.kubectl cluster-info`

MicroK8s is easy to use and comes with plenty of [Kubernetes add-ons](https://microk8s.io/docs/addons) you can enable or disable.

## Master node and leaf nodes
Now that you have MicroK8s installed on all boards, pick one is to be the master node of your cluster.

On the chosen one, run the following command:

`sudo microk8s.add-node`

This command will generate a connection string in the form of `<master_ip>:<port>/<token>`.

Adding a node
Now, you need to run the join command from another Pi you want to add to the cluster:

`microk8s.join <master_ip>:<port>/<token>`

For example:

`microk8s.join 172.19.181.254:25000/JHpbBYMIevZSAMnmjMHmFwanrOYCWZLu`

You should be able to see the new node in a few seconds on the master with the following command:

`microk8s.kubectl get node`

For each new node, you need to run the microk8s.add-node command on the master, copy the output, then run microk8s.join <master node output> on the leaf.

### Removing nodes


To remove a node, run the following command on the master:

`sudo microk8s remove-node <node name>`

The name of nodes are available on the master by running the `microk8s.kubectl get node` command.

Alternatively, you can leave the cluster from a leaf node by running:

`sudo microk8s.leave`

# Some urls I've getted info:
* [https://ubuntu.com/tutorials/how-to-kubernetes-cluster-on-raspberry-pi#1-overview](https://ubuntu.com/tutorials/how-to-kubernetes-cluster-on-raspberry-pi#1-overview)

* [https://clusterhat.com/](https://clusterhat.com/)
* [https://www.cnx-software.com/2021/12/09/raspberry-pi-zero-2-w-power-consumption/](https://www.cnx-software.com/2021/12/09/raspberry-pi-zero-2-w-power-consumption/)
