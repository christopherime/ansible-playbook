# ansible-playbooks

A selection of personal Ansible playbooks for automating common tasks.

## table of contents

- [ansible-playbooks](#ansible-playbooks)
  - [table of contents](#table-of-contents)
  - [01\_update\_server](#01_update_server)
  - [02\_update\_date-time](#02_update_date-time)
  - [03\_assign\_ip](#03_assign_ip)
  - [04\_plex\_back\_config](#04_plex_back_config)
  - [05\_proxmox\_create\_template](#05_proxmox_create_template)
  - [06\_deploy\_k3s-cluster](#06_deploy_k3s-cluster)
  - [07\_deploy\_plex-server](#07_deploy_plex-server)
  - [08\_uninstall\_k3s-all-nodes](#08_uninstall_k3s-all-nodes)

## 01_update_server

This playbook updates the target host with the latest packages and security updates.

## 02_update_date-time

This playbook updates the date and time on the target host.

## 03_assign_ip

This playbook assigns IP addresses to all machines in the inventory. It uses a predefined gateway, netmask, and DNS servers, as well as an IP address range to determine the machine's IP address. If the IP address is not already defined, the playbook generates a new IP address within the specified range. The playbook then creates a netplan configuration file on the target machines using a template file, netplan.j2. Finally, it applies the new netplan configuration to set the assigned IP addresses and network configurations for the machines.

## 04_plex_back_config

This playbook deploys the Plex Media Server and creates a backup of the /plex directory on the target host. It first creates a tarball of the /plex directory, both asynchronously and synchronously, and then creates and mounts an NFS share. Once the asynchronous tarball creation is complete, the playbook copies the tarball to the mounted NFS share. Afterward, it unmounts the NFS share and removes the tarball from the target host, leaving the backup on the NFS share.

## 05_proxmox_create_template

This playbook automates the process of creating an Ubuntu VM template in Proxmox. It downloads the specified Ubuntu cloud image, customizes the image with user creation, SSH key injection, timezone setting, and package updates. The playbook then creates and configures a new VM template in Proxmox, which can be used to spawn new VM instances with the pre-configured settings. The VM ID is stored in a local file and incremented for each run, ensuring unique IDs for multiple templates.

## 06_deploy_k3s-cluster

This playbook installs and configures a k3s Kubernetes cluster across multiple nodes. It ensures that required packages are installed on all nodes, downloads the k3s installation script, and installs k3s on the control-plane and worker nodes. The control-plane nodes are initialized first, and a cluster token is generated and stored in the inventory. The worker nodes then join the cluster using this token, connecting to the control-plane nodes via the specified server URL.

## 07_deploy_plex-server

This playbook deploys Plex Media Server using Docker on a target host. It ensures the required packages are installed, Docker is running, and it creates the necessary configuration directories and NFS Docker volumes before deploying the Plex container.

Required variables for the host_vars file:
| Variable | Description |
| --- | --- |
| nfs_server | The IP address of the NFS server |
| nfs_media_path | The path on the NFS server where the media files are stored |
| nfs_data_path | The path on the NFS server where data files are stored |
| plex_config_path | The local path on the target host for Plex configuration files |
| nfs_mount_path | The mount point for the NFS backup on the target host |

## 08_uninstall_k3s-all-nodes

This playbook uninstalls the k3s cluster from all nodes in the inventory. It executes the k3s server and agent uninstall scripts on all targeted hosts, ensuring complete removal of k3s components from both server and agent nodes. Any errors during the uninstallation process are ignored to ensure the playbook continues its execution.
