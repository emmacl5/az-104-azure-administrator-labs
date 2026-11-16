# Lab 08 — Manage Virtual Machines

## Objective

Deploy and manage Azure virtual machines, configure availability
zones, and practice VM administration.

## Environment

- Subscription: Azure subscription 1
- Resource group: `az104-rg8`
- Region: East US
- Virtual network: `vnet-eastus-1`
- Subnet: `snet-eastus-1` (`172.16.0.0/24`)

## Task 1 — Deploy Azure Virtual Machines Across Availability Zones

### VM 1: az104-vm0

| Setting | Configuration |
|---|---|
| Name | `az104-vm0` |
| Operating system | Windows Server 2025 Datacenter: Azure Edition |
| Size | `Standard_D2s_v3` |
| Availability zone | Zone 1 |
| OS disk | Premium SSD |
| Public IP | None |
| Public inbound ports | None |
| Security | Trusted Launch, Secure Boot, vTPM |
| Boot diagnostics | Enabled with managed storage |
| Auto-shutdown | 19:00 Helsinki time |

### Verification

- [x] Azure deployment completed successfully.
- [x] VM power state verified as Running.
- [x] Availability zone verified as Zone 1.
- [x] VM agent status verified as Ready.
- [x] Screenshot evidence captured.

### Evidence

![az104-vm0 running in Availability Zone 1](screenshots/01-az104-vm0-running.png)

### VM 2: az104-vm1

| Setting | Configuration |
|---|---|
| Name | `az104-vm1` |
| Operating system | Windows Server 2025 Datacenter: Azure Edition |
| Size | `Standard_D2as_v6` |
| Availability zone | Zone 2 |
| Virtual network | `vnet-eastus-1` |
| Subnet | `snet-eastus-2` (`172.16.1.0/24`) |
| OS disk | Premium SSD |
| Public IP | None |
| Public inbound ports | None |
| Security | Trusted Launch, Secure Boot, vTPM |
| Boot diagnostics | Enabled with managed storage |
| Auto-shutdown | 19:00 Helsinki time |

### Verification — VM 2

- [x] Azure deployment completed successfully.
- [x] VM power state verified as Running.
- [x] Availability zone verified as Zone 2.
- [x] VM agent status verified as Ready.
- [x] Screenshot evidence captured.

### Evidence — VM 2

![az104-vm1 running in Availability Zone 2](screenshots/02-az104-vm1-running.png)

### Lessons Learned

- Azure VM size availability can differ between availability zones.
- `Standard_D2s_v3` was unavailable in Zone 2, so an alternative SKU was selected.
- Both VMs were deployed in East US across separate availability zones.
- Both VMs use the same virtual network but different subnets.
- Neither VM has a public IP address.

## Task 2 — Resize an Azure Virtual Machine (Vertical Scaling)

### Objective

Practice vertical scaling by increasing the CPU and RAM of `az104-vm0`, then restoring its original size.

### Resize Results

| Property | Original | Scaled Up | Final |
|---|---|---|---|
| VM size | `Standard_D2s_v3` | `Standard_D4s_v3` | `Standard_D2s_v3` |
| vCPUs | 2 | 4 | 2 |
| RAM | 8 GiB | 16 GiB | 8 GiB |
| Availability Zone | Zone 1 | Zone 1 | Zone 1 |

### Procedure

1. Opened `az104-vm0` in Azure Portal.
2. Attempted to resize using **Availability + scale → Size**.
3. The **Resize** button remained disabled.
4. Stopped and deallocated the VM.
5. Confirmed inherited **Owner** permissions.
6. Verified available resize options using Azure CLI.
7. Resized the VM to `Standard_D4s_v3`.
8. Restored the original size, `Standard_D2s_v3`.
9. Verified the final power state was `VM deallocated`.

### Azure CLI Commands

```bash
az vm list-vm-resize-options --resource-group az104-rg8 --name az104-vm0

az vm resize --resource-group az104-rg8 --name az104-vm0 --size Standard_D4s_v3

az vm show --resource-group az104-rg8 --name az104-vm0 --query "hardwareProfile.vmSize" --output tsv

az vm resize --resource-group az104-rg8 --name az104-vm0 --size Standard_D2s_v3

az vm get-instance-view --resource-group az104-rg8 --name az104-vm0 --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" --output tsv

```

### Verification

- [x] Target size confirmed through Azure CLI.
- [x] VM resized to `Standard_D4s_v3` (4 vCPUs, 16 GiB RAM).
- [x] Original size `Standard_D2s_v3` restored.
- [x] Final provisioning state: `Succeeded`.
- [x] Final power state: `VM deallocated`.

### Lessons Learned

- Azure Portal's Resize button remained disabled, but Azure CLI resizing succeeded.
- VM size compatibility depends on the disk controller type.
- Deallocating a VM can make additional resize options available.
- Deallocated VMs do not incur compute charges, although managed disks can still incur charges.
- Vertical scaling changes a VM's CPU and memory allocation.


## Remaining Exercises

- [x] Deploy second VM in Availability Zone 2.
- [x] Resize a virtual machine.
- [ ] Manage VM data disks.
- [ ] Deploy a Virtual Machine Scale Set.
- [ ] Configure autoscaling.
- [ ] Document cleanup and lessons learned.

## Status

In progress.
