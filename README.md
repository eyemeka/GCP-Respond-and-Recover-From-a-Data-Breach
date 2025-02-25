# GCP: Respond and Recover From a Data Breach

<h2 id="step1">Activity overview</h2>
This lab is part of the capstone project. In this lab, you’ll apply your knowledge of cloud cybersecurity to identify and remediate vulnerabilities.

You’ll be given a scenario, and a set of tasks to complete in Google Cloud Security Command Center. These tasks will require you to use your skills to work to analyze and remediate active vulnerabilities relating to a security incident, answer questions about the vulnerabilities, and complete challenges that will assess your cloud cybersecurity skills.

There are also a number of challenges in the lab. A challenge is a task where you will be asked to complete the task on your own without instructions.

By successfully completing this lab, you will demonstrate your ability to identify, prioritize, and remediate security vulnerabilities and misconfigurations within the cloud environment. These are essential skills to enhance the security posture of Google Cloud environments, reducing the risk of data breaches, unauthorized access, and other security incidents.

<h2 id="step2">Scenario</h2>
For the last year, you've been working as a junior cloud security analyst at Cymbal Retail. Cymbal Retail is a market powerhouse currently operating 170 physical stores and an online platform across 28 countries. They reported $15 billion in revenue in 2022, and currently employ 80,400 employees across the world.

Cymbal Retail boasts a vast customer base with a multitude of transactions happening daily on their online platform. The organization is committed to the safety and security of its customers, employees, and its assets, ensuring that its operations meet internal and external regulatory compliance expectations in all the countries it operates in.

Recently, the company has experienced a massive data breach. As a junior member of the security team, you’ll help support the security team through the lifecycle of this security incident. You'll begin by identifying the vulnerabilities related to the breach, isolate and contain the breach to prevent further unauthorized access, recover the compromised systems, remediate any outstanding compliance related issues, and verify compliance with frameworks.

Here’s how you'll do this task: <strong>First</strong> you’ll examine the vulnerabilities and findings in Google Cloud Security Command Center. <strong>Next</strong>, you’ll shut the old VM down, and create a new VM from a snapshot. <strong>Then</strong>, you’ll evoke public access to the storage bucket and switch to uniform bucket-level access control. <strong>Next</strong>, you’ll limit the firewall ports access and fix the firewall rules. <strong>Finally</strong>, you’ll run a report to verify the remediation of the vulnerabilities.
<h2></h2>
<h2>Task 1. Analyze the data breach and gather information</h2>
One morning, the security team detects unusual activity within their systems. Further investigation into this activity quickly reveals that the company has suffered a massive security breach across its applications, networks, systems, and data repositories. Attackers gained unauthorized access to sensitive customer information, including credit card data, and personal details. This incident requires immediate attention and thorough investigation. The first step towards understanding the scope and impact of this breach is to gather information and analyze the available data.

In this task, you'll examine the vulnerabilities and findings in Google Cloud Security Command Center to determine how the attackers gained access to the data, and which remediation steps to take.

<strong>First</strong>, navigate to the Security Command Center to view an overview of the active vulnerabilities.
<ol>
 	<li>In the Google Cloud console, in the <strong>Navigation menu</strong> (<img src="https://cdn.qwiklabs.com/tkgw1TDgj4Q%2BYKQUW4jUFd0O5OEKlUMBRYbhlCrF0WY%3D" alt="navigation_menu" />), click <strong>Security &gt; Overview</strong>. The Security Command Center Overview page opens.</li>
 	<li>Scroll down to <strong>Active vulnerabilities</strong>. This provides an overview of current security vulnerabilities or issues that need attention within the Google Cloud environment.</li>
 	<li>Select the <strong>Findings By Resource Type</strong> tab. The security findings or vulnerabilities based on the type of cloud resource affected (e.g., instances, buckets, databases) are organized. By reviewing active vulnerabilities and findings by resource type, you can prioritize and address security issues effectively.</li>
</ol>
<img class="aligncenter size-full wp-image-1762" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-153.png" alt="" width="1588" height="818" />

<strong>Next</strong>, navigate to the PCI DSS report.
<ol start="4">
 	<li>In the <strong>Security Command Center</strong> menu, click <strong>Compliance</strong>. The Compliance page opens.</li>
 	<li>In the <strong>Google Cloud compliance standards</strong> section, click <strong>View details</strong> in the <strong>PCI DSS 3.2.1</strong> tile. The PCI DSS 3.2.1 report opens.</li>
 	<li>Click on the <strong>Findings</strong> column to sort the findings and display the active findings at the top of the list.</li>
</ol>
The Payment Card Industry Data Security Standard (PCI DSS) is a set of security requirements that organizations must follow to protect sensitive cardholder data. As a retail company that accepts and processes credit card payments, Cymbal Retail must also ensure compliance with the PCI DSS requirements, to protect cardholder data.

<img class="aligncenter size-full wp-image-1763" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-154.png" alt="" width="1600" height="813" />

As you examine the PCI DSS 3.2.1 report, notice that it lists the rules that are non-compliant, which relate to the data breach:
<ul>
 	<li><strong>Firewall rule logging should be enabled so you can audit network access</strong>: This medium severity finding indicates that firewall rule logging is disabled, meaning that there is no record of which firewall rules are being applied and what traffic is being allowed or denied. This is a security risk as it makes it difficult to track and investigate suspicious activity.</li>
 	<li><strong>Firewall rules should not allow connections from all IP addresses on TCP or UDP port 3389</strong>: This high severity finding indicates that the firewall is configured to allow Remote Desktop Protocol (RDP) traffic for all instances in the network from the whole internet. This is a security risk as it allows anyone on the internet to connect to the RDP port on any instance in the network.</li>
 	<li><strong>Firewall rules should not allow connections from all IP addresses on TCP or SCTP port 22</strong>: This high severity finding indicates that the firewall is configured to allow Secure Shell (SSH) traffic to all instances in the network from the whole internet. SSH is a protocol that allows secure remote access to a computer. If an attacker can gain access to a machine through SSH, they could potentially steal data, install malware, or disrupt operations.</li>
 	<li><strong>VMs should not be assigned public IP addresses</strong>: This high severity finding indicates that a particular IP address is actively exposed to the public internet and is potentially accessible to unauthorized individuals. This finding is considered a potential security risk because it could allow attackers to scan for vulnerabilities or launch attacks on the associated resource.</li>
 	<li><strong>Cloud Storage buckets should not be anonymously or publicly accessible</strong>: This high severity finding indicates that there is an Access Control List (ACL) entry for the storage bucket that is publicly accessible which means that anyone on the internet can read files stored in the bucket. This is a high-risk security vulnerability that needs to be prioritized for remediation.</li>
 	<li><strong>Instances should not be configured to use the default service account with full access to all Cloud APIs</strong>: This medium severity finding indicates that a particular identity or service account has been granted full access to all Google Cloud APIs. This finding is considered a significant security risk because it grants the identity or service account the ability to perform any action within the Google Cloud environment, including accessing sensitive data, modifying configurations, and deleting resources.</li>
</ul>
Since you're focusing on identifying and remediating the issues related to the security incident, please disregard the following findings as they do not relate to the remediation tasks you’re completing:
<ul>
 	<li><strong>VPC Flow logs should be Enabled for every subnet VPC Network</strong>: There are a number of low severity findings for Flow Logs disabled. This indicates that Flow Logs are not enabled for a number of subnetworks in the Google Cloud project used for this lab. This is a potential security risk because Flow Logs provide valuable insights into network traffic patterns, which can help identify suspicious activity and investigate security incidents.</li>
 	<li>
<ul>
 	<li><strong>Basic roles (Owner, Writer, Reader) are too permissive and should not be used</strong>: This medium severity finding indicates that primitive roles are being used within the Google Cloud environment. This is a potential security risk because primitive roles grant broad access to a wide range of resources.</li>
 	<li><strong>An egress deny rule should be set</strong>: This low severity finding indicates that no egress deny rule is defined for the monitored firewall. This finding raises potential security concerns because it suggests that outbound traffic is not restricted, potentially exposing sensitive data or allowing unauthorized communication.</li>
</ul>
The following table pairs the rules listed in the report with their corresponding findings category. This will assist you when examining the findings according to resource type later:</li>
</ul>
<table>
<tbody>
<tr>
<th>Findings category</th>
<th>Rule</th>
</tr>
<tr>
<td>Firewall rule logging disabled</td>
<td>Firewall rule logging should be enabled so you can audit network access</td>
</tr>
<tr>
<td>Open RDP port</td>
<td>Firewall rules should not allow connections from all IP addresses on TCP or UDP port 3389</td>
</tr>
<tr>
<td>Open SSH port</td>
<td>Firewall rules should not allow connections from all IP addresses on TCP or SCTP port 22</td>
</tr>
<tr>
<td>Public IP address</td>
<td>VMs should not be assigned public IP addresses</td>
</tr>
<tr>
<td>Public bucket ACL</td>
<td>Cloud Storage buckets should not be anonymously or publicly accessible</td>
</tr>
<tr>
<td>Full API access</td>
<td>Instances should not be configured to use the default service account with full access to all Cloud APIs</td>
</tr>
<tr>
<td>Flow logs disabled</td>
<td>VPC Flow logs should be Enabled for every subnet VPC Network</td>
</tr>
<tr>
<td>Primitive roles used</td>
<td>Basic roles (Owner, Writer, Reader) are too permissive and should not be used</td>
</tr>
<tr>
<td>Egress deny rule not set</td>
<td>An egress deny rule should be set</td>
</tr>
</tbody>
</table>
Overall, these findings indicate a critical lack of security controls and non-compliance with essential PCI DSS requirements; they also point to the vulnerabilities associated with the data breach.

<img class="aligncenter size-full wp-image-1764" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-155.png" alt="" width="1600" height="664" />



The following active findings pertaining to the storage bucket should be listed:
<ul>
 	<li><strong>Public bucket ACL</strong>: This finding is listed in the PCI DSS report, and indicates that anyone with access to the internet can read the data stored in the bucket.</li>
 	<li><strong>Bucket policy only disabled</strong>: This indicates that there is no explicit bucket policy in place to control who can access the data in the bucket.</li>
 	<li><strong>Bucket logging disabled</strong>: This indicates that there is no logging enabled for the bucket, so it will be difficult to track who is accessing the data</li>
</ul>
These findings indicate that the bucket is configured with a combination of security settings that could expose the data to unauthorized access. You'll need to remediate these findings by removing the public access control list, disabling public bucket access, and enabling the uniform bucket level access policy.

I Also Checked The Google <strong>Google compute instance</strong> (screenshot below)



<img class="aligncenter size-full wp-image-1765" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-156.png" alt="" width="1600" height="814" />



The following active findings that pertain to the virtual machine named <strong>cc-app-01</strong> should be listed:
<ul>
 	<li><strong>Malware bad domain</strong>: This finding indicates that a domain known to be associated with malware was accessed from the google.compute.instance named cc-app-01. Although this finding is considered to be of low severity, it indicates that malicious activity has occurred on the virtual machine instance and that it has been compromised.</li>
 	<li><strong>Compute secure boot disabled</strong>: This medium severity finding indicates that secure boot is disabled for the virtual machine. This is a security risk as it allows the virtual machine to boot with unauthorized code, which could be used to compromise the system.</li>
 	<li><strong>Default service account used</strong>: This medium severity finding indicates that the virtual machine is using the default service account. This is a security risk as the default service account has a high level of access and could be compromised if an attacker gains access to the project.</li>
 	<li><strong>Public IP address</strong>: This high severity finding is listed in the PCI DSS report and indicates that the virtual machine has a public IP address. This is a security risk as it allows anyone on the internet to connect to the virtual machine directly.</li>
 	<li><strong>Full API access</strong>: This medium severity finding is listed in the PCI DSS report, and indicates that the virtual machine has been granted full access to all Google Cloud APIs.</li>
</ul>


These findings indicate the virtual machine was configured in a way that left it very vulnerable to the attack.

TO REMEDIATE THIS

To remediate these findings you'll shut the original VM (cc-app-01) down, and create a VM (cc-app-02) using a clean snapshot of the disk. The new VM will have the following settings in place:
<ul>
 	<li>No compute service account</li>
 	<li>Firewall rule tag for a new rule for controlled SSH access</li>
 	<li>Secure boot enabled</li>
 	<li>Public IP address set to None</li>
</ul>
<strong>FOR Google Compute Firewalls</strong>

<img class="aligncenter size-full wp-image-1766" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-157.png" alt="" width="1600" height="459" />



The following active findings should be listed that pertain to the firewall:
<ul>
 	<li><strong>Open SSH port</strong>: This high severity finding indicates that the firewall is configured to allow Secure Shell (SSH) traffic to all instances in the network from the whole internet.</li>
 	<li><strong>Open RDP port</strong>: This high severity finding indicates that the firewall is configured to allow Remote Desktop Protocol (RDP) traffic to all instances in the network from the whole internet.</li>
 	<li><strong>Firewall rule logging disabled</strong>: This medium severity finding indicates that firewall rule logging is disabled. This means that there is no record of which firewall rules are being applied and what traffic is being allowed or denied.</li>
</ul>
These findings are all listed in the PCI DSS report and highlight a significant security gap in the network's configuration. The lack of restricted access to RDP and SSH ports, coupled with disabled firewall rule logging, makes the network highly vulnerable to unauthorized access attempts and potential data breaches. You'll need to remediate these by removing the existing firewall overly broad rules, and replacing them with a firewall rule that allows SSH access only from the addresses that are used by Google Cloud's IAP SSH service



<strong>NOW THAT YOU HAVE ANALYZED THE SECURITY VULNERABILITIES, IT’S TIME TO WORK ON REMEDIATING THE REPORT FINDINGS.</strong>


<h2 id="step5">Task 2. Fix the Compute Engine vulnerabilities</h2>
TO Fix the Compute Engine Vulnerabilities, I shut down the vulnerable VM <strong>cc-app-01</strong>, and create a new VM from a snapshot taken before the malware infection. VM snapshots are effective in restoring the system to a clean state, and ensures that the new VM will not be infected with the same malware that compromised the original VM.



After I shut down the vulnerable VM <strong>cc-app-01</strong>

I created another VM create a new VM from a snapshot. This snapshot has already been created as part of Cymbal Retail's long term data backup plan

In Creating The New VM <strong>cc-app-02, </strong>I Ensured I created a Network tag to apply a Firewall rule to This Specific VM.

I Also Ensured That The External IPv4 Address was set to <strong>NONE</strong> SO That It Will Not Be Open To A Potential Attack From The Internet

<img class="aligncenter size-full wp-image-1767" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-160.png" alt="" width="1600" height="355" />

After Creating the New VM, <strong>I, Needed </strong>turn Secure Boot on for the new VM <strong>cc-app-02</strong> to address the <strong>Secure Boot disabled</strong> finding.

<img class="aligncenter size-full wp-image-1768" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-161.png" alt="" width="1600" height="369" />



I Restarted The <strong>cc-app-02</strong> VM instance and the <strong>Secure Boot disabled</strong> finding has been <strong>remediated.</strong>


<h2 id="step6">Task 3. Fix Cloud Storage bucket permissions</h2>
In this task, you'll revoke public access to the storage bucket and switch to uniform bucket-level access control, significantly reducing the risk of data breaches. By removing all user permissions from the storage bucket, you can prevent unauthorized access to the data stored within.

<img class="aligncenter size-full wp-image-1769" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-162.png" alt="" width="1600" height="621" />



There is a <strong>myfile.csv</strong> file in the publicly accessible bucket. This is the file that contains the sensitive information that was dumped by the malicious actor.

<strong> TO ADDRESS THE PUBLIC BUCKET ACL FINDING;</strong>

I Prevented Public Access To This Bucket

<img class="aligncenter size-full wp-image-1770" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-164.png" alt="" width="1600" height="762" />



I Also Switched the access control to uniform and remove permissions for the <strong>allUsers</strong> principals from the storage bucket to enforce a single set of permissions for the bucket and its objects. I Also needed to ensure that users who rely on basic project roles to access the bucket won't lose their access.

In the Above Task. I effectively prevented public access to the bucket, switched to uniform bucket-level access control, and removed all user permissions, addressing the <strong>Public bucket ACL</strong>, <strong>Bucket policy only disabled</strong>, and <strong>Bucket logging disabled</strong> findings.


<h2 id="step7">Task 4. Limit firewall ports access</h2>
In this task, you'll restrict access to RDP and SSH ports to only authorized source networks to minimize the attack surface and reduce the risk of unauthorized remote access.

Exercise extreme caution before modifying overly permissive firewall rules. The rules may be allowing legitimate traffic, and improperly restricting it could disrupt critical operations. In this lab, ensure the Compute Engine virtual machine instances tagged with target tag "cc" remain accessible via SSH connections from the Google Cloud Identity-Aware Proxy address range (35.235.240.0/20). To maintain uninterrupted management access, create a new, limited-access firewall rule for SSH traffic before removing the existing rule allowing SSH connections from any address.


<h2 id="step8">Task 5. Fix the firewall configuration</h2>
In this task, you'll delete three specific VPC firewall rules that are responsible for allowing unrestricted access to certain network protocols, namely ICMP, RDP, and SSH, from any source within the VPC network. Then, you'll enable logging on the remaining firewall rules.

<img class="aligncenter size-full wp-image-1771" src="https://www.businesstoks.com.ng/wp-content/uploads/2024/10/Screenshot-166.png" alt="" width="1600" height="838" />



After Deleting three specific VPC firewall rules that are responsible for allowing unrestricted access to certain network protocols, I Enabled Logs for the New Firewall Rule I created to restrict Access To SSH.

By customizing firewall rules and enabling logging, I have addressed the <strong>Open SSH port</strong>, <strong>Open RDP port</strong>, and <strong>Firewall rule logging disabled</strong> findings. The new firewall rule better protects the network and improves network visibility.
