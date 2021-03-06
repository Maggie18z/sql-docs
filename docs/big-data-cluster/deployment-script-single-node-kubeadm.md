---
title: Deploy with a bash script to a single node kubeadm cluster
titleSuffix: SQL Server big data clusters
description: Use a bash deployment script to deploy a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] to a single node kubeadm cluster.
author: mihaelablendea 
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
---

# Deploy with a bash script to a single node kubeadm cluster

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

In this tutorial, you use a sample bash deployment script to deploy a single node Kubernetes cluster using  kubeadm and a SQL Server big data cluster on it.  

## Prerequisites

- A vanilla Ubuntu 18.04 or 16.04 **server** virtual or physical machine. All dependencies are set up by the script, and you run the script from within the VM.

  > [!NOTE]
  > Using Azure Linux VMs is not yet supported.

- VM should have at least 8 CPUs, 64-GB RAM, and 100 GB of disk space. After pulling all big data cluster Docker images, you will be left with 50 GB for data and logs to use across all components.

- Update existing packages using commands below to ensure that the OS image is up-to-date.

   ``` bash
   sudo apt update&&apt upgrade -y
   sudo systemctl reboot
   ```

## Recommended virtual machine settings

1. Use static memory configuration for the virtual machine. For example, in Hyper-V installations do not use dynamic memory allocation but instead allocate the recommended 64 GB or higher.

1. Use checkpoint or snapshot capability in your hyper visor so that you can roll back the virtual machine to a clean state.


## Instructions to deploy SQL Server big data cluster

1. Download the script on the VM you are planning to use for the deployment.

   ```bash
   curl --output setup-bdc.sh https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/setup-bdc.sh
   ```

2. Make the script executable with the following command.

   ```bash
   chmod +x setup-bdc.sh
   ```

3. Run the script (make sure you are running with *sudo*)

   ```bash
   sudo ./setup-bdc.sh
   ```

   When prompted, provide your input for the password to use for the following external endpoints: controller, SQL Server master, and gateway. The password should be sufficiently complex based on existing rules for SQL Server password. The controller username defaults to *admin*.

4. Set up an alias for the **azdata** tool.

   ```bash
   source ~/.bashrc
   ```

5. Refresh alias setup for azdata.

   ```bash
   azdata --version
   ```

## Cleanup

The [cleanup-bdc.sh](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/cleanup-bdc.sh) script is provided as convenience to reset the environment if necessary. However, we recommend that you use a virtual machine for testing purposes and use the snapshot capability in your hypervisor to roll back the virtual machine to a clean state.

## Next steps

To get started with using big data clusters, see [Tutorial: Load sample data into a SQL Server big data cluster](tutorial-load-sample-data.md).
