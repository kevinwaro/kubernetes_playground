# Kubernetes playground

## Description:

A set of ansible playbook ran by Vagrant to setup a 3 nodes Kubernetes cluster. This is useful when you want to discover and
practice Kubernetes administrarion.

## Usage:

First, clone the repo:

   ```
   git clone https://github.com/kevinwaro/kubernetes_playground && cd kubernetes_playground
   ```

Then let Vagrant create and provision the vms:

   ```
   vagrant up master node1 node2
   ```

Once everything is provisionned, you can ssh to the master node and start playing with Kubernetes:

   ```
   vagrant ssh master
   $ kubectl get nodes
   NAME     STATUS   ROLES    AGE     VERSION
   master   Ready    master   8m39s   v1.17.3
   node1    Ready    <none>   5m19s   v1.17.3
   node2    Ready    <none>   114s    v1.17.3
   ```

## Credits:

* author: kevinwaro
* contact: kevinwaro@yahoo.fr

## License:

This project is licensed under the GNU GPLv3 License - see the [License.md](License.md) file for details
