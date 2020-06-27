OCP-Numa
=========
a OCP cluster with multi-numa workers that have multiple nics on different numa nodes

Steps
------
1. install ocp with kcli
use kcli_parameters.yaml
this will create workers with 2 numa nodes and additional nics
2. when worker is deployed use
virsh edit <worker domain>
and add pxb and attach nics to appropriate bridge as you wish.
this will add 2 pci-expander-bridges, one to each numa node
so that when nics are attached to the bridge, it is attach to that numa node
see example in libvirt_patch.xml
note that you can copy the pxb definition but will need to manually patch the nics
3. enable host-device cni plugin
I was simply following the explanations:
https://docs.openshift.com/container-platform/4.2/networking/multiple_networks/configuring-host-device.html
the gist being:
patch cluster network CR with
cluster_network_patch.yaml
you can use oc patch -f cluster_network_patch.yaml
* TODO: fix network so it is a pool of nics
4. create pod that uses that
see pod.yaml example
