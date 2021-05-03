# Ansible playbook to prepare Windows OS image for OpenShift using Packer

Note: This playbook has been based on this repo: https://github.com/openshift/windows-machine-config-operator/tree/master/docs/vsphere_ci

## Goals
This playbook uses `Packer` to prepare a Windows image to be used with Windows Nodes on OpenShift.

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

Example of use:

```shell
ansible-playbook ocp-win-prepare -e vsphere_server=vcsa.rhbr-lab.com -e vsphere-password=***** -e ssh_pub_key='ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQD1EERwUoWjOgEwQOTuxe9bZ+boCHn4oOy1CeQZB6M9nsNNJtejq20WiPV9Vwj0kVCjx52L9OcfBG7+C2jY0Juq4fMtgsnKy02zLTHb/m4JqkmjIr5o+dD6hpf6mwS31bYOXWjyaHPXHAaTCBfL84sFCC1avlvl2UwWppP25RfEnyLzsNQxlpSWLVTU+LiB3z8dREiGO/o03Vr9qVwNCmJI+SJ4duTSy9FUgzBJ9tTeYCUIYqn8p7l22/IXpOCk3QKWML82bVK8X1oDoYfgnDbsqBB2KYzJhrB3l1xdibiXhc1yGjg+SRvWB047LufRb+VVt0M5nIq3IDcq1axeh5OgNJOIDAXMSa+/t66e1jXrlf+WA4tQ3Xc3zRuK0nl5PCtzgJ6Zaw3sBWo9T4Kp2i8+ci+n+qPDot6bH3hIDqSdwMoVH/q0RC6y9X6Fq7C4yd6hBA2oGgxvn8P+1JQ26sgrrZ0nNOkhkKl3BN2dTVyqbOj4/IX4hjhWOOdiNwexU2SbZasdfasdfasdfasdfasdfasdfasdf'
```

## Notes
Consider the following notes when trying to use this playbook:

- This playbook has been tested with Red Hat Enterprise Linux 8 and Fedora 33, it may not work with other linux distributions not based on RHEL. If you intent to try it using other distributions and have suggestion to make it more portable, please feel free to propose a PR - I will be happy to analyse and approve it as soon as I can! :)
