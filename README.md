# Ansible playbook to prepare Windows OS image for OpenShift (using Packer)

Note: This playbook was based on this repo: https://github.com/openshift/windows-machine-config-operator/tree/master/docs/vsphere_ci

## Goals
This playbook uses `Packer` to prepare a Windows image to be used with Windows Nodes on OpenShift.

![Prepare Windows Image](imgs/run.gif)
## How to use

You should define the following variables to run it:
- vcenter_win_iso_path: Path where Windows ISO file is located (e.g.: "[datastore1] ISOs/WinServer2019.iso").
- vcenter_vmwaretools_iso_path: Path where the VMware Tools ISO file is located (e.g.: "[] /usr/lib/vmware/isoimages/windows.iso").
- vsphere_cluster: Name of the cluster on vSphere.
- vsphere_datacenter: Name of the datacenter on vSphere.
- vsphere_datastore: Name of the datastore on vSphere.
- vsphere_network: Name of the network on vSphere (default: "VM Network").
- vsphere_network_card: Name of the network card type (e.g.: vmxnet3, e1000e, etc.).
- vsphere_server: URL of the VMware vCenter (e.g.: vcsa.rhbr-lab.com).
- vsphere_user: User to logon on vCenter.
- vsphere_password: Passwords to logon on vCenter.
- win_vm_name: Name of the VM template will be created as a result of this process (default: "win-ocp-image-template")
- win_vm_cpu: Amount of CPU (default: "4")
- win_vm_disk: Amount of hard disk (default: "128000")
- win_vm_mem: Amount of RAM (default: "16384")
- win_admin_passwd: Password that will be used to set Administrator password at Windows VM (default: "admin123")
- ssh_pub_key: SSH public key that will be injected into the Windows VM.

Instructions to use it:

1. Clone this repo into your workspace (it needs to have access to vCenter API URL).

```shell
$ git clone https://github.com/giofontana/ocp-windows-image-prepare.git
```

2. Edit the variables according to your environment. You can do that either by editing the `vars` section of `ocp-win-prepare-image.yml` file:

```yaml
---
- name: Prepare windows image for OpenShift
  hosts: localhost
  vars:
    packer_url: "https://releases.hashicorp.com/packer/1.6.6/packer_1.6.6_linux_amd64.zip"

    vcenter_win_iso_path: "[datastore1] ISOs/WinServer2019.iso"
    vcenter_vmwaretools_iso_path: "[] /usr/lib/vmware/isoimages/windows.iso"
    vsphere_cluster: "cluster"
    vsphere_datacenter: "Datacenter"
    vsphere_datastore: "datastore1"
    vsphere_folder: "Plataformas"
    vsphere_network: "VM Network"
    vsphere_network_card: "e1000e" #vmxnet3, e1000e, etc.
    vsphere_server: "vcenter_url"
    vsphere_user: "Your_username"
    vsphere_password: "Your_password"    

    win_vm_name: "win-ocp-image-template"
    win_vm_cpu: "4"
    win_vm_disk: "120000"
    win_vm_mem: "16384"
    win_admin_passwd: "admin123"    
    ssh_pub_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCcfW4O3CYg0IENfw5QplkHr57b+ntfLiBmyHaXbgQSq4F5Jplz2rQSMYIY/2Ki8EgD4zTsjif5zjiluggLgVc48l5dKLXFYscs+c+hTbQbsu5/4P641HylFADk8ijYs1WXZ+JdTFnTb2B/404+JnusM+MAZ6inHoBseKup4MifBN6TQVb7uCJLrbNluqMeE0qhRcQtHrqqfWiDOvJYVpGjiJG4ZzaIvbBLcuvtCAxg7QnGAQrcOL2gnc6i7WR6g5hJWXNctSpv4XkcKVyMsJIfqmdxZ5AmOlUJ8KkSDN0POo7y27sg6qeuRfb9r+nJlfDjwDRL9fLrUAAvSVtEM4uV"
```

Or you may not edit them in the `ocp-win-prepare-image.yml` and define as extra-vars directly at ansible-playbook command. Below you can see an example of the ansible-playbook command

```shell
# using extra vars
ansible-playbook ocp-win-prepare-image.yml -e "vcenter_win_iso_path='[datastore1] ISOs/WinServer2019.iso'" -e vsphere_cluster='cluster' -e vsphere_datacenter='Datacenter' -e vsphere_datastore='datastore1' -e vsphere_folder='Plataformas' -e vsphere_server='vcsa.rhbr-lab.com' -e vsphere_user='Administrator@rhbr-lab.com' -e vsphere_password='****' -e "ssh_pub_key='ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQD1EERwUoWjOgEwQOTuxe9bZ+boCHn4oOy1CeQZB6M9nsNNJtejq20WiPV9Vwj0kVCjx52L9OcfBG7+C2jY0Juq4fMtgsnKy02zLTHb/m4JqkmjIr5o+dD6hpf6mwS31bYOXWjyaHPXH****************************'"


# if you set vars at ocp-win-prepare-image.yml, just run the ansible playbook
ansible-playbook ocp-win-prepare-image.yml
```

## Notes
Consider the following notes when trying to use this playbook:

- This playbook has been tested with Red Hat Enterprise Linux 8 and Fedora 33, it may not work with other linux distributions not based on RHEL. If you intent to try it using other distributions and have suggestion to make it more portable, please feel free to propose a PR - I will be happy to analyse and approve it as soon as I can! :)
