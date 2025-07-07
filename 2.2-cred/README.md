# Workshop Exercise: Managing Inventories and Credentials in Ansible Automation Controller

## Objective
This workshop provides practical experience with managing inventories and credentials in  automation controller. You’ll learn how to navigate a preloaded inventory, understand its structure, and set up machine credentials for accessing managed hosts.

## Table of Contents
1. [Introduction to Inventories](#1-introduction-to-inventories)
2. [Exploring the 'Workshop Inventory'](#2-exploring-the-workshop-inventory)
3. [Add "Mock Dynamic Inventory"](#3-add-mock-dynamic-inventory)
4. [Understanding Machine Credentials](#4-understanding-machine-credentials)
5. [Additional Credential Types](#5-additional-credential-types)
6. [Add Vault Credential](#6-add-vault-credential)
7. [Conclusion](#7-conclusion)

### 1. Introduction to Inventories
In automation controller, inventories define and organize the hosts your playbooks will target. They can be static (a fixed list of hosts) or dynamic (sourced from external systems).

### 2. Exploring the _Workshop Inventory_
The _Workshop Inventory_ is preloaded in your lab environment, representing a static inventory configuration.

- **Accessing the Inventory:** Navigate to **Automation Execution → Infrastructure → Inventories** in the web UI, and select _Workshop Inventory_.
- **Viewing Hosts:** Navigate to **Automation Execution → Infrastructure → Hosts** to see the predefined hosts, similar to those in a traditional Ansible inventory file.

![Hosts](images/hosts.png)

### 3. Add "Mock Dynamic Inventory"
The Workshop Inventory above is pre-built, however this workshop is designed to mimic the fact that inventories will be dynamically pulled from the hyperscaler/hypervisor that terraform builds from the main.tf file.  To mimic this, we will dynamically pull the inventory from a project.

In a true integration with AAP and Terraform, you will be able to dynamically pull inventories from the hypervisor and group them based on tags, which those groups would then be targeted for post-provisioning configuration.

* Go to **Automation Execution → Infrastructure → Inventories** click the **Create inventory** button. Fill in the form:

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Name</td>
     <td>Mock Dynamic Inventory</td>
   </tr>
   <tr>
     <td>Organization</td>
     <td>Default</td>
   </tr>
 </table>

* Click **Create inventory** button

### 4. Understanding Machine Credentials
Machine credentials are essential for establishing secure SSH connections to managed hosts.

- **Accessing Credentials:** Navigate to **Automation Execution → Infrastructure→ Credentials** and select _Workshop Credentials_.
- **Credential Details:** The 'Workshop Credentials' is configured with:
  - **Credential Type:** Machine (for SSH).
  - **Username:** A predefined user, such as `ec2-user`.
  - **SSH Private Key:** Encrypted, providing secure access to hosts.

### 5. Additional Credential Types
Automation controller supports over 30 different credential types for various automation tasks. Here are a few common ones:

- **Network Credentials:** For managing network devices.
- **Source Control Credentials:** For accessing source control systems.
- **Amazon Web Services (AWS) Credentials:** For integrating with AWS services.

These credential types enhance the flexibility and security of your automation efforts.

### 6. Add Vault Credential

* Go to **Automation Execution → Infrastructure → Credentials** click the **Create Credential** button. Fill in the form:

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Name</td>
     <td>vault</td>
   </tr>
   <tr>
     <td>Organization</td>
     <td>Default</td>
   </tr>
   <tr>
     <td>Credential Type</td>
     <td>Vault</td>
   </tr>
   <tr>
     <td>Vault Password</td>
     <td>R3dh4t1!</td>
   </tr>
 </table>

* Click **Create credential** button

### 7. Conclusion
This workshop introduces the essential concepts of inventories and credentials within Ansible Automation Controller. Mastering these components is critical for effectively managing your automation environments and ensuring secure access to your infrastructure.

---
**Navigation**
<br>[Previous Exercise](../2.1-intro) | [Next Exercise](../2.3-projects)  
[Click here to return to the Ansible for Red Hat Enterprise Linux Workshop](../README.md#section-2---ansible-tower-exercises)

