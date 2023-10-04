# Cloud Migration Strategy and Security Review

#Introduction

This document is designed to showcase the technical implementation of the various services to effectively migrate on-prem environment to Amazon Web Services(AWS) to improve its security posture. The documents put emphasizes several and how they can be mitigated using various AWS services. Once the application migrates to the AWS cloud, it will be highly secure, scalable, resilient, and compliant.

Major problems in the existing application - 
1.	Improper Account Authorization
2.	Highly vulnerable to DDoS attacks
3.	No Patching Strategy
4.	Slow Streaming, Downloads and Order Processing
5.	No Backup Strategy
6.	Improper data protection
7.	Non-Compliant application
8.	Insecure coding practices 

Various tools and services leveraged to improve the security posture
Account Authorization	AWS Identity and Access Management
DDoS attacks prevention	Amazon Shield Advanced, Route 53, AWS WAF
Patch Management	AWS Patch Manager
Fast Streaming, Downloads and Order Processing	AWS CloudFront and S3 bucket
Backup	AWS Backup Service and AWS S3 Glacier
Data protection	Amazon Macie
Compliant	PCI and PII Compliant
Secure coding practices 	AWS CodeGuru

New Architecture Proposal 
The below-advanced architecture level diagram shows how the on-prem infrastructure looks after it is successfully migrated to the AWS cloud.     
              
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/be43520f-6860-47d9-9a33-f1de671a9aed)




Migration Steps 
Discovery	Prepare both existing and future system architecture 
Create a migration inventory based on all the functional and non-functional requirements
Design	Prepare a migration strategy for every component followed by a high-level migration plan.
Development	Make a list of step-by-step migration plans and test plans, Automation scripts, and ready-to-deploy applications in CloudFormation,
Testing and refinement	Create testing reports and redefine the automation scripts, migration plans and testing plans.
Implementation	Deploy all the components in the AWS Cloud.

Issue 1 – Account Authorization
Account Authorization is one of the most critical aspects of security. We can use the web service AWS Identity and Access Management (IAM) to securely manage access to AWS resources. We will be able to manage permissions that govern which AWS resources users can access from a single location with IAM.
Recommendation
Currently, the firm does not have an Identity and Access Management service and every user has the access to run privileged commands on the web server which is very hazardous for the firm. We are going to leverage Role-based access control, the principle of least privilege, MFA, and password management policy. 

Roles and Responsibilities

Roster Name - 	Roles - 	Policies
Chuck -	Founder of the firm - Read Only Access
Bob - Chief Operating Officer - Read Only Access
Kenny -	Chief Information Security Officer - Administrator Access
Hunter - Chief Information Officer - Power User Access (This role can be modified based on the access a person) 
Sydney - Web Developer - Power User Access(This role can be modified based on the access a person)
Tim - System Administrator - System Administrator



Implementation 

Setting up password management:
Credential Management allows the root user to establish parameters for the user to construct a password, thus preventing the user from creating a weak password and making it more difficult for the attacker. 
     Creating a custom password policy:

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/4ec19bcb-4155-479b-92dc-e5eb56834192)

•	In the navigation pane of the IAM management console, go to Account settings > Edit
 
•	Then click on ‘Custom’ and checkmark the policies that you want to be set (refer to the screenshot below).

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/28d361f0-cf41-4288-a55e-f567d187c0ba)

  
•	After setting the policies, select ‘Save changes’ and then click on ‘set custom’ to finally impose those policies.

MFA (Multi-Factor Authentication)
MFA (Multi-Factor Authentication) adds an extra layer of security by allowing users to enter a security code to successfully authenticate to the application.
To setup MFA for another user-  
•	Navigate to the IAM console and then select Users.
•	Select the name of the user for whom you want to add MFA.
•	Navigate to Account name > Security credentials in the top right corner. You will see an MFA section as shown in the screenshot.

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/97e07c22-b5f7-4acd-bb99-512efff87b81)

•	In the MFA section, select Assign MFA User > Select MFA Device > Device Name > Authenticator App.
•	Then select the app which you want the user to use and follow the instructions accordingly.
•	After the successful completion of all the steps, the details about the assigned MFA device can be seen under the MFA tab (refer to the screenshot below

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/49cab067-d0b9-4972-b332-467dad877aaf)

•	Note: The same process can be followed by the root user to create an MFA for himself/herself.

Other Points to consider while configuring IAM 

•	Implement Role-based access control. Provide only limited access to the users that they need to perform their jobs. Create different policies based on the different privileged users and assign those policies to a group. Follow the ‘principle of least privilege.’
•	Only provide high-privilege access to a minimum number of users.
•	Implement services to collect log records that can analyze who has made requests to access the resource.
•	Don’t use root users for daily tasks.
•	Implement tools for monitoring purposes. Trigger an alert to prevent the threat, especially in the case of privileged accounts.
•	Perform regular audit assessments on the cloud identity management framework.
•	Regularly check and eliminate roles, policies, permissions, users, and credentials that are not used.
•	Implement a strong password policy that includes a password rotation policy.
•	Modify access control policies based on conditions for accessing your cloud resources.

Issue 2 - DDOS Resiliency
A DDOS also known as Denial A Denial of Service (DoS) attack is a malicious attempt to make an application unavailable to users by inundating it with a large volume of requests or by conducting infrastructure and application layer attacks.

Recommendation
As we already know that the application is highly vulnerable to DDOS attacks and has also experienced multiple compromised DDOS attempts from a rival dojo run by Daniel LaRusso who is a persistent threat to the firm's Operations. AWS shield advanced, AWS WAF and AWS Route 53 are used to guard our application from attacks targeted at different layers. These services act as entry-level checks and protect our resources from DDoS attacks.  

Amazon Route 53 
Amazon Route 53 is configured by registering a domain with Amazon Route 53 and setting up Route 53 to respond to DNS queries that resolve to a static website. 

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/4c2b663f-6d17-44c6-8cfb-a6ae83620c15)






Implementation 
The below diagram summarizes how AWS Shield Standard, Amazon Route 53 and AWS WAF work in tandem to protect the application from DDoS attacks. 

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/e70555ed-052c-4dd1-bfad-89f0333489f0)

 

Points to consider while configuring Amazon Route 53
•	Make use of data plane functions such as health checks and Application recovery controller for DNS failover and app recovery.
•	The TTL values for DNS records are important. The recommended range is 60 to 172800 seconds.
•	Make use of CNAME records to point one domain to another. 
•	Make use of Advanced DNS routing.
•	Consider the use of DNS delegation, DNS change propagation, and size of DNS response. 





AWS WAF
AWS WAF is configured by creating Web ACL for the CloudFront distribution. Once the rules are defined in the ACL, filtering the traffic for HTTP and HTTPS requests will be made easier by the AWS WAF.

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/e02748bf-bb11-40ce-be2b-3427a1fd3d41)

Implementation 
The below diagram summarizes how AWS Shield Standard, Amazon Route 53 and AWS WAF work in tandem to protect the application from DDoS attacks. 

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/bbf9c297-4b5a-45a8-afa0-e00f8af613ad)
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/a73a2a79-bb94-4042-86db-90c73231ecf0)



 
Points to consider while configuring AWS WAF
•	Implement enforcement policy for the edge network layer.
•	Implement application layer policy enforcement in the load balancer
•	Make use of AWS Managed Rules to implement application load balancer origin and edge network.
•	Implement rules for public and private application load balancers using AWS Managed WAF 
•	Set up CloudWatch Alarms and lambda responders for logging.

AWS Shield Advanced
AWS Shield Advanced can be configured by subscribing to Shield Advanced in AWS. This feature can be used to protect resources such as, Global Accelerator, Amazon EC2 Elastic IP addresses, Route 53, Elastic Load Balancing (ELB) load balancers, and CloudFront distributions.

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/e2910fa1-c129-48be-b4e6-d4877938a862)


Implementation 
The below diagram summarizes how AWS Shield Standard, Amazon Route 53 and AWS WAF work in tandem to protect the application from DDoS attacks. 

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/7ef8c92a-51fb-4954-b5b8-989e98389b60)

 
Points to consider while configuring AWS Advanced Shield
•	Integration with AWS WAF provides additional support against application layer DDoS.
•	Integration with Amazon Route 53 health checks to inform event detection and mitigation.
•	Clubbing of the intended resources into groups for enhanced detection and mitigation of the complete group.
•	Make use of centralized management of Advanced Shield protections by AWS Firewall Manager.
•	Make use AWS Shield Response Team (SRT) whenever necessary.

Issue – 3: Patch Management
Patching is one of the indispensable security strategies to mitigate errors (also referred to as “vulnerabilities” or “bugs”) in software. Any loopholes or security flaws can keep the application open to numerous attacks by external threats.

Recommendations
Once deployed in the AWS cloud, we can leverage “AWS system manager,” which is a service to deploy security patches in the software and operating systems. This service will make use of patch baseline, patch group and maintenance window to regularly update all the firm's resources.

Implementation
•	Maintain asset inventory containing data about various resources such as Virtual Machines, Databases, and Machine Configurations.
•	Define a patch group and maintenance window based on different environments (Development, Production, Testing) and Operating Systems.
•	Create a patch baseline (approved patches) for an OS.
•	Test the patching solution in a non-production environment before deploying it into production. Verify if everything works properly after the patching is delivered.
•	Automate the patching process using Patch Manager with the help of the patching group and maintenance window to deploy patches in future.
•	Send mass communication to all the users and the internal team about the patching activity.
•	Prepare a dashboard insight and patch compliance report using asset inventory data to cater for the compliance requirement.

 

 

Issue 4 - Optimize Slow Streaming, Downloads and Order Processing
Slow downloads, streaming, and order processing are aggravating, and they not only waste your time but also negatively impact the customer experience. This could also result in potential clients switching to a rival, costing the company business.
Recommendations
The firm's customers have been complaining about slow streaming, downloads, and order processing. We need to implement a better strategy to cater for the needs of users. CloudFront distribution and S3 buckets would come in handy for us to resolve this issue and also provide a better application experience to our loyal customers.

CloudFront Distribution
Our web content can be delivered more quickly and the load on our origin servers will be automatically reduced when Amazon CloudFront is set up with Amazon Simple Storage Service (S3).

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/69e869b1-701c-4e61-98ce-0aac4b89bf20)

 
Implementation
To do this, we can utilize Amazon CloudFormation and launch an S3 bucket at scale, store our static files there, and then distribute those files to our users over the CloudFront CDN (CDN). In order to decrease latency and offload capacity from your origin, the CloudFront edge locations will cache and distribute our content closer to your users. Our content and application will be more secure and efficient as a result of CloudFront's restriction of access to our S3 bucket only to just CloudFront endpoints. 
 
Points to consider while configuring AWS CloudFront Distribution
•	Ensure that AWS Cloudfront web distributions are enabled with automatic object compression.
•	Verify that CloudFront distribution's geo-restriction feature is activated.
•	Check that the AWS CloudFront CDN solution is being used for quick and safe delivery of web content.
•	Ensure that no insecure SSL protocols are used by CloudFront origins.
•	Ensure that AWS WAF is linked with your Cloudfront CDN distributions.
•	Ensure that access logging is turned on for AWS Cloudfront CDN distributions.
•	Verify if distributions of AWS CloudFront employ enhanced security protocols for HTTPS connections.
•	Ensure encrypted communication between a CloudFront distribution and the origin.

Issue 5 – Backup Management
A backup strategy is planned to centralize and computerize the backup of resources across AWS services. It is one of the quintessential parts of maintaining a good security posture. With AWS backup, we can decide on backup policies, and backup retention periods, and it saves a lot of time during a cyber-attack.

Recommendation
The application does not have a backup strategy which helps in restoring information, ensure business continuity, and defend against devastating IT crises. It is highly likely that during a cyber-attack, an attack can completely erase data resulting in the loss of business. We are going to leverage the AWS Backup service and AWS S3 Glacier to build and store a backup plan and use those backups in case of any emergency. Through backup, the firm can restore their on-demand content and customer data at any point in time without affecting business continuity.


 
Implementation
  ![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/25207f9d-ddcf-43df-940f-c034acaa34d6)
AWS Backup and AWS S3 Glacier
A better backup strategy can be made by combining AWS Backup with S3 Glacier. Both live streaming and on-demand training sessions are available through the app. As a result, S3 Glacier is utilized as a backup for relational databases in a virtual private cloud to store training videos and other material that can be accessed immediately. Additionally, the AWS backup service offers a routine for automatically backing up all the services accessible through the AWS cloud infrastructure.

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/dadcd29c-5fa6-41e5-ab81-00af954d8da4)


 
Points to consider while configuring AWS Backup and AWS S3 Glacier
•	Define policies on what, when and how to take backup. i.e., Backup of on-demand content, Customer information.
•	Ensure to have encryption on the backups. The encryption key could be either Platform or Customer managed.
•	Perform full backups followed by periodic incremental backups. Helps in minimizing storage costs as well.
•	Make sure to enable a Backup vault lock that prevents the deletion of data.
•	Decide to store backups across many areas. When a region is down and a crucial application needs to be operating, this is crucial. Additionally, there are occasions when we must keep backups a certain amount of distance from our production data to meet business continuity or compliance requirements.
•	Establish a backup retention period.
•	Backups can be connected with other tools to manage metrics, examine dashboards, and monitor workloads.
•	Use Audit Manager to check that our AWS Backup policies comply with the various established controls.

Issue 6 – PII Protection and PCI Compliance 
Personal Identifiable Information and Credit Card information is one of the most indispensable data inside an application. Customers entrust us with their personal information, thus we have a duty to secure it at all costs. Individuals may suffer serious consequences as a result of the loss of PII, including identity theft or other illicit uses of the data.

Recommendation
The application is processing credit card data and also stores customer PII (name, phone, email, address, and additional details about the customer). We need to ensure that all Personal Identifiable Information and Credit Card information is stored safely either in rest or transit. For this purpose, we will leverage Amazon Macie to discover and protect our sensitive data.

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/763000f5-dc00-46d4-a81b-0538d53be44b)

Amazon Macie
The Simple Storage Service (Amazon S3) data estate is protected by Amazon Macie, which automatically assesses and keeps track of the buckets' security and access control. Macie creates a finding for you to examine and correct as necessary if it notices a potential problem with the security or privacy of your data, such as a bucket that becomes publicly accessible. 

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/9d967209-7b42-4a16-bafc-89a0834bce8d)

Implementation
Create access permissions to the Amazon Macie console and API activities first. When everything is finished, Macie will immediately start keeping an accurate inventory of all of our S3 buckets in the current Region.

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/3bb82218-e6eb-4c28-a653-26df13f26d4a)

Points to consider while configuring Amazon Macie
•	Investigating unsuccessful Macie scans of S3 items
•	Ensuring S3 and KMS resource policy compliance
•	Enabling the Macie service-linked IAM role to scan S3 objects.
•	Use SCPs to prevent Macie from being changed without permission.
•	IAM roles for Amazon Macie provisioning
•	In all Regions where you have workloads using S3 buckets, enable Macie.
•	Enable the Amazon Macie and Security Hub integration so that Macie results can be sent to Security Hub (enabled by default).
•	To compile Macie's findings into a single Region, enable Security Hub Region aggregation.
•	Import logs from AWS CloudWatch Logs to offer individualized alerting for outcomes of sensitive data discovery jobs using Macie.
•	In the Macie settings, activate the Auto-enable option.


PCI Compliance
The PCI Security Standards Council is in charge of managing the Payment Card Industry Data Security Standard (PCI DSS), a private information security standard and AWS helps in protecting credit card information using PCI compliance.
Recommendation
The firm has not been compliant in their on-prem environment however AWS has a feature that can be implemented to make the application PCI compliant. 
Implementation
1.	Before we get started, it is necessary to enable Amazon’s Security Hub. To do so, simply visit the “Enable Security Hub” feature on the console. While doing so, we will see an “Enable PCI DSS v3.2.1” checkbox which should be checked. It is also recommended to enable “AWS Foundational Security Best Practices v1.0.0”.
2.	Now, navigate to the PCI DSS v.3.2.1 webpage and a list of “All enabled controls” can be seen which will also show the status of controls (i.e. passed, failed, disabled). Refer to the screenshot below.
 
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/e6a565db-a6e6-4a70-8ee4-f098ad79f418)

3.	 From here, a description of every control can be seen by simply clicking on the control name and then it can be analyzed whether we want to enable or disable that specific control or not.
A crucial point to remember is that the controls cannot confirm whether your systems comply with the PCI DSS standard. Therefore, we must make sure that we fix the security tests that failed. To do so, 
a.	the very first step should be to prioritize the list of those failed checks which are to be fixed. 
b.	Then, visit the details page of control by selecting the control you wish to correct.
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/7cf98b4f-9644-48ff-9d81-36dfe0e24c55)
 
c.	Follow the link for Remediation instructions, then adhere to the detailed remediation instructions for each failed discovery.


Issue 7 – Insecure Coding Practice
An essential part of maintaining an application's security is application security testing. The organization can automate the integration of security at each stage of the software development lifecycle, from initial design to integration, testing, deployment, and software delivery, by integrating a security plugin in the pipeline.

Recommendation
We are going to leverage AWS CodeGuru to identify the vulnerabilities during the initial stages of the software development process and fix those issues before moving into production.
Implementation 
Integration of Amazon CodeGuru reviewer (CLI) with our Jenkins Continuous Integration & Continuous Delivery (CI/CD) pipeline is very straight forward. CodeGuru Reviewer combines machine learning (ML) and automated reasoning to spot typical coding errors as well as security flaws and wasteful uses of AWS APIs and SDKs. Additionally, we have our source code on Github where we may integrate CodeGuru reviewer.
In the build stage of the pipeline, we configure the build tool to perform the code build and security analysis and once this is completed, we configure the CodeGuru Reviewer CLI to generate the recommendations based on the review and fix those vulnerabilities based on the recommendations. 
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/d16940c7-4f06-4089-bc3f-d3bec1692a42)
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/4ede9c6b-4ba3-4694-8281-a03d7a4e1ebc)
![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/9de1585b-da54-41b7-859e-c0f3382d4fc7)

 Creating a Jenkins Pipeline job
 
 

Reviewing the CodeGuru Reviewer recommendations
When the build is complete, choosing Jenkins to produce a history for the most recent build job will allow us to see the CodeGuru Reviewer review results. Browse to Workspace output after that. There are HTML and JSON formats for the output. 

![image](https://github.com/4tul-rawat/Consulting-in-Cloud-Security/assets/130515502/0190e117-9923-4619-8afa-b2fb43157e2f)


References
Amazon CodeGuru. (n.d.). Retrieved from Amazon AWS: https://aws.amazon.com/blogs/devops/automating-detection-of-security-vulnerabilities-and-bugs-in-ci-cd-pipelines-using-amazon-codeguru-reviewer-cli/
AWS WAF. (n.d.). Retrieved from Amazon AWS: https://aws.amazon.com/blogs/security/defense-in-depth-using-aws-managed-rules-for-aws-waf-part-1/ 
CloudFront. (n.d.). Retrieved from Amazon AWS: https://aws.amazon.com/cloudfront/getting-started/S3/ 
DDOS Resilient. (n.d.). Retrieved from Amazon AWS: https://aws.amazon.com/blogs/security/how-to-protect-dynamic-web-applications-against-ddos-attacks-by-using-amazon-cloudfront-and-amazon-route-53/ 
Enable virtual MFA. (n.d.). (Amazon AWS) Retrieved from https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-iam-user
Password account policies. (n.d.). (Amazon AWS) Retrieved from https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html
Route 53. (n.d.). Retrieved from Amazon AWS: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/best-practices-dns.html 


