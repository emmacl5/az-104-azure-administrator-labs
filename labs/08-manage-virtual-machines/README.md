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

Not started.

## Remaining Exercises

- [ ] Deploy second VM in Availability Zone 2.
- [ ] Resize a virtual machine.
- [ ] Manage VM data disks.
- [ ] Deploy a Virtual Machine Scale Set.
- [ ] Configure autoscaling.
- [ ] Document cleanup and lessons learned.

## Status

In progress.
