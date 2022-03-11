# rhel8-kubernetes-deployment
This role deploys kubernetes onto rhel8 machines in a single controlplane/multi-worker topology.<br>
It also deploys the Calico SDN by default using these [manifests](https://docs.projectcalico.org/manifests/calico.yaml)

### My environment Setup:
I'm using the rhel qcow2 (cloud) images for the deployment, so there are a few things that you may have to do that I didn't: <br>
- disable swap in '/etc/fstab'
- install firewalld package and start/enable the service

I manually copied the ssh-keys from my control-machine to each of the nodes.

**This is not a production ready deployment**<br>
I set selinux to `permissive` mode to get through the installation. I did enable the necessary ports in firewalld.

### Sample Inventory File 
```yaml
controlplane:
  hosts:
    ctlplane.example.com:
      ansible_host: 192.168.122.220

workers:
  hosts:
    worker0.example.com:
      ansible_host: 192.168.122.149
    worker1.example.com:
      ansible_host: 192.168.122.244
```

### Sample `site.yml` playbook to call the role
```yaml
---
- name: Kubernetes Installer
  hosts: all
  remote_user: root
  roles:
    - rhel8-kubernetes-deployment
```
