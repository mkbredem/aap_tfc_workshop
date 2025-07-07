# Workshop Exercise - Projects & Job Templates

## Table Contents

* [Objective](#objective)
* [Guide](#guide)
* [Setup Git Repository](#setup-git-repository)
* [Create the Project](#create-the-project)
* [Create a Job Template and Run a Job](#create-a-job-template-and-run-a-job)
* [Challenge Lab: Check the Result](#challenge-lab-check-the-result)
* [What About Some Practice?](#what-about-some-practice)

## Objective

A **job template** allows you to run an automation job. In order to run any type of automation, a job template must be created. A job template consists of knowning the following information:

Inventory: On what hosts should the job run?

Credentials What credentials are needed to log into the hosts?

Project: Where is the playbook?

Playbook: What playbook to use?

This exercise covers:

* Understanding and using/defining Ansible automation controller Job Templates
* Using a Job Template to create more Job Templates.

## Guide

### Add "Setup Job Template"
The first template we will add runs a playbook that actually automates AAP itself using configuration as code.  This job template will build out other resources (credentials, inventories, and job templates) that can optionally be used needed for the rest of the workshop, as well as provide solutions or shortcuts.  

### Create “Setup” job template:
* Go to **Automation Execution → Templates** click the **Create Template** button. Fill in the form:

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Name</td>
     <td>Setup</td>
   </tr>
   <tr>
     <td>Organization</td>
     <td>Default</td>
   </tr>
   <tr>
     <td>Project</td>
     <td>Terraform Workshop Project</td>
   </tr>
   <tr>
     <td>Inventory</td>
     <td>Demo Inventory</td>
   </tr>
   <tr>
     <td>Playbook</td>
     <td>setup_workshop_solutions.yml</td>
   </tr>
   <tr>
     <td>Execution Environment</td>
     <td>Default execution environment</td>
   </tr>
   <tr>
     <td>Playbook</td>
     <td>setup_workshop_solutions.yml</td>
   </tr>
   <tr>
     <td>Credentials</td>
     <td>Demo Credential | Vault</td>
   </tr>
   <tr>
     <td>Extra Vars</td>
     <td>
       <pre><code>controller_hostname: controller.&lt;CHANGE&gt;.sandbox&lt;CHANGE&gt;.opentlc.com
aap_password: R3dh4t1!
aap_service_account_password: R3dh4t1!
student_account: admin</code></pre>
     </td>
   </tr>
 </table>

* Click **Create job template** 
* Click **Launch template** click to run the Setup JT

Once it is done running, see the following resources were created for you:
* Go to **Automation Execution → Templates**
![Solutions JTs](images/solution_job_templates.png)

* **Solution1 - Add Job Templates:** We will be adding more job templates, to mimic a post provisioning workflow (base config -> application deployment → load balancer -> CMDB update -> ITSM tracking, etc…).  You can use this Solution1 to make sure all the Job Templates are built correctly.
* **Solution2 - complete config and deploy workflow:** wonce the job templates are added, you will be stitching them together in a Workflow Job Template.  Again, this may take more time than the lab will allow, so you can complete the Workflow using this Solution2.
* **Solution3 - Decommission Workflow Job Template:** This playbook adds additional job templates and completes an additional Workflow that is used to mimic the decommissioning process. Similar to Solution1 and Solution2 combined, but for the Decommissioning workflow.

* Go to **Automation Execution → Infrastructure → Credentials**
![AAP Credentials](images/aap_credential.png)

* **AAP Credential:** this credential is used by the Solution[1:3] to authenticate into the Automation Controller API, similar to the Extra Vars you pasted into **Setup** job template in **Step 6**.

## Add all the job templates needed for post provisioning configuration

### Base Config
The logical place to start would be to perform a base configuration of the newly provisioned servers.  This may include things like hardening configurations, installing and activating security agents, and other compliance requirements needed on all servers:
Automation Execution → Templates →  Create Template
Complete form as follows:
Name: 3 - Apply Base Config
Description: Common Server Configuration (agents, hardening, etc..)
Inventory: Mock Dynamic Inventory
Project: Terraform Workhsop Project
Playbook: common_config.yml
Execution environment: Default Execution Environment
Credentials: 
Workshop Credentials | Machine
Vault | Vault
Click “Create job template” button
### Create CR
In order to track resources being created in ITSM, we need to create a change request:
Automation Execution → Templates →  Create Template
Complete form as follows:
Name: 1 - Create ITSM Change Request
Description: 
Inventory: Demo Inventory
Project: Terraform Workhsop Project
Playbook: servicenow_catalog_create.yml
Execution environment: Default Execution Environment
Credentials: Demo Credential | Machine
Click “Create job template” button
### Deploy Application
Upon successful completion of the Common Config, we will need to run additional configurations in order to deploy the application that will run on the server.
Automation Execution → Templates →  Create Template
Complete form as follows:
### Create “Setup” job template:
* Go to **Automation Execution → Templates** click the **Create Template** button. Fill in the form:

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Name</td>
     <td></td>
   </tr>
   <tr>
     <td>Organization</td>
     <td></td>
   </tr>
   <tr>
     <td>Inventory</td>
     <td>Demo Inventory</td>
   </tr>
   <tr>
     <td>Project</td>
     <td>Terraform Workshop Project</td>
   </tr>
   <tr>
     <td>Playbook</td>
     <td></td>
   </tr>
   <tr>
     <td>Execution Environment</td>
     <td>Default execution environment</td>
   </tr>
  <td>Credentials</td>
  <td>
    Workshop Credentials | Machine<br>
    Vault | Vault<br>
  </td>
 </table>

* Click **Create job template** 

Name: 4 - Deploy Web App 1
Description: Common Server Configuration (agents, hardening, etc..)
Inventory: Mock Dynamic Inventory
Project: Terraform Workhsop Project
Playbook: common_config.yml
Execution environment: Default Execution Environment
Credentials: 
Workshop Credentials | Machine
Vault | Vault
Click “Create job template” button
### Add Hosts to Load Balancer
Then we will need to add the host(s) to a load balancer.  Since this is targeting a device external to the inventory we are configuring directly, we will need to create a separate job template for this piece as well.
Automation Execution → Templates →  Create Template
Complete form as follows:
Name: 5 - Add Hosts to LB
Description: 
Inventory: Demo Inventory
Project: Terraform Workhsop Project
Playbook: add_ip_lb.yml
Execution environment: Default Execution Environment
Credentials: Demo Credential | Machine
Click “Create job template” button
### Add hosts to CMDB
Now that the server is configured and the app deployoed, we will need to add it to our CMDB database, our single source of truth.
Automation Execution → Templates →  Create Template
Complete form as follows:
Name: 6 - Create CMDB Records
Description: 
Inventory: Demo Inventory
Project: Terraform Workhsop Project
Playbook: servicenow_cmdb_create.yml
Execution environment: Default Execution Environment
Credentials: Demo Credential | Machine
Click “Create job template” button
### Update CR
We will need to periodically update the change request we created earlier with notes to indicate progress (i.e. common config complete, app deploy complete, or even app deploy fail, etc…).
Automation Execution → Templates →  Create Template
Complete form as follows:
Name: 7 - Update ITSM Change Request
Description: 
Inventory: Demo Inventory
Project: Terraform Workhsop Project
Playbook: servicenow_catalog_update.yml
Execution environment: Default Execution Environment
Credentials: Demo Credential | Machine
Click “Create job template” button
### Create Incident
What if one of the configuration, deploy, adding to loadbalancer job fail?  We will need to create an incident for investigation and/or remediation, to finish and test the configurations.
Automation Execution → Templates →  Create Template
Complete form as follows:
Name: 8 - Create ITSM Incident
Description: 
Inventory: Demo Inventory
Project: Terraform Workhsop Project
Playbook: servicenow_incident_create.yml
Execution environment: Default Execution Environment
Credentials: Demo Credential | Machine
Click “Create job template” button
Solution 1: Run Job Template to build additional resources (CaC)
Automation Execution → Templates → Launch Solution1 - Add Job Templates (click rocket)
After it is finished running, you will see all the job templates from the previous exercise:
Automation Execution → Templates
Note: you may seem some duplicates if there was a typo in the Name of the job template



> **Tip**
>
> Copy and paste the Name value into the Job Templates, so if a Solution is used, the job templates you created will simply be updated or skipped if no changes are identified, otherwise, you will have similarly named redundant job templates when a solution runs.

> **Tip**
>
> if at any point you’ve had enough practice building job templates, or if you get the idea, you can skipp to [Solution1](#solution-1) to have ansible create all these tasks for you.


To configure and use this repository as a **Source Control Management (SCM)** system in automation controller you have to create a **Project** that uses the repository

### Create the Project

* Go to **Automation Execution → Projects** click the **Create Project** button. Fill in the form:

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Name</td>
     <td>Workshop Project</td>
   </tr>
   <tr>
     <td>Organization</td>
     <td>Default</td>
   </tr>
   <tr>
     <td>Execution Environment</td>
     <td>Default execution environment</td>
   </tr>
   <tr>
     <td>Source Control Type</td>
     <td>Git</td>
   </tr>
 </table>

 Enter the URL into the Project configuration:

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Source Control URL</td>
     <td><code>https://github.com/ansible/workshop-examples.git</code></td>
   </tr>
   <tr>
     <td>Options</td>
     <td>Select Clean, Delete, Update Revision on Launch to request a fresh copy of the repository and to update the repository when launching a job.</td>
   </tr>
 </table>

* Click **Create project**


The new project will be synced automatically after creation. But you can also do this manually: Sync the Project again by selecting the 'Sync project' blue button.

![Project Sync](images/project_sync.png)


After starting the sync job, go to the **Jobs** view, and you'll find the job doing the project update.

### Create a Job Template and Run a Job

A job template allows you to run an automation job. In order to run any type of automation, a job template must be created. A job template consists of knowning the following information:

* **Inventory**: On what hosts should the job run?

* **Credentials** What credentials are needed to log into the hosts?

* **Project**: Where is the playbook?

* **Playbook**: What playbook to use?

To create a Job Template, go to the **Automation Execution -> Templates** view,click the **Create template** button and choose **Create job template**.

> **Tip**
>
> Remember that you can often click on the question mark with a circle to get more details about the field.

 <table>
   <tr>
     <th>Parameter</th>
     <th>Value</th>
   </tr>
   <tr>
     <td>Name</td>
     <td>Install Apache</td>
   </tr>
   <tr>
     <td>Job Type</td>
     <td>Run</td>
   </tr>
   <tr>
     <td>Inventory</td>
     <td>Workshop Inventory</td>
   </tr>
   <tr>
     <td>Project</td>
     <td>Workshop Project</td>
   </tr>
   <tr>
     <td>Playbook</td>
     <td><code>rhel/apache/apache_install.yml</code></td>
   </tr>
   <tr>
     <td>Execution Environment</td>
     <td>Default execution environment</td>
   </tr>
   <tr>
     <td>Credentials</td>
     <td>Workshop Credential</td>
   </tr>
   <tr>
     <td>Limit</td>
     <td>web</td>
   </tr>
   <tr>
     <td>Options</td>
     <td>tasks need to run as root so check **Privilege Escalation**</td>
   </tr>
 </table>

* Click **Create job template**

You can start the job by directly clicking the blue **Launch template** button, or by clicking on the rocket in the Job Templates overview. After launching the Job Template, you are automatically brought to the job overview where you can follow the playbook execution in real time.

Template Details
![template details](images/template_detail.png)

Job Run
![job_run](images/job_run.png)

Since this might take some time, have a closer look at all the details provided:

* All details of the job template like inventory, project, credentials and playbook are shown.

* Additionally, the actual revision of the playbook is recorded here - this makes it easier to analyse job runs later on.

* Also the time of execution with start and end time is recorded, giving you an idea of how long a job execution actually was.

* Selecting **Output** shows the output of the running playbook. Click on a node underneath a task and see that detailed information are provided for each task of each node.

After the Job has finished go to the main **Jobs** view: All jobs are listed here, you should see directly before the Playbook run an Source Control Update was started. This is the Git update we configured for the **Project** on launch\!

### Solution 1

Time for a little challenge:

* Use an ad hoc command on both hosts to make sure Apache has been installed and is running.

You have already been through all the steps needed, so try this for yourself.

> **Tip**
>
> What about `systemctl status httpd`?


> **Warning**
>
> **Solution Below**

* Go to **Automation Execution → Infrastructure →  Inventories** → **Workshop Inventory**

* In the **Automation Execution → Infrastructure → Inventories → Workshop Inventory**, select the **Hosts** tab and select `node1`, `node2`, `node3` and click **Run Command**

Within the **Details** window, select **Module** `command`, in **Arguments** type `systemctl status httpd` and click **Next**.

Within the **Execution Environment** window, select **Default execution environment** and click **Next**.

Within the **Credential** window, select **Workshop Credentials** and click **Next**.

Review your inputs and click **Finish**.

Verify that the output result is as expected.

> **Tip**
>
> The output of the results is displayed once the command has completed.

---
**Navigation**
<br>
[Previous Exercise](../2.2-cred) - [Next Exercise](../2.4-surveys)

[Click here to return to the Ansible for Red Hat Enterprise Linux Workshop](../README.md#section-2---ansible-tower-exercises)
