# proposal
 
Proposal: Automated Lifecycle Management for AWS Sandbox and Classroom accounts Resources
Author: Sai Praneeth Sattenapalli.
Date: 05/20/2026























1.	Purpose:
The objective of this initiative is to implement an automated lifecycle management system for AWS sandbox and classroom environments to:
•	Eliminate manual cleanup efforts
•	Reduce cloud resource waste and cost
•	Ensure all resources are temporary by default
•	Provide a safe and controlled deletion process
________________________________________

2.	Problem Statement:
Currently, sandbox environments tend to accumulate:
•	Unused or forgotten resources
•	Stopped instances and orphaned storage
•	Long-running test environments

This results in:
•	Increased cloud costs
•	Operational overhead for cleanup
•	Lack of ownership visibility
A consistent automated approach is required to manage these resources effectively.
________________________________________
3.	Proposed Solution
We propose implementing an automated lifecycle system that manages every resource from creation to deletion without requiring manual intervention.
The system will:
1.	Automatically assign an expiration date to resources
2.	Monitor resources on a daily basis
3.	Notify users before expiration
4.	Stop (quarantine) resources before deletion
5.	Delete resources after a defined grace period
6.	Optionally optimize cost using scheduling
________________________________________
4.	Resource Lifecycle Process
4.1	Resource Creation
When a resource is created:
•	The system automatically assigns:
•	ExpiresOn = Current Date + 30 days
	LifecycleState = Active
	The user is expected to provide:
	OwnerEmail (optional)
	If OwnerEmail is not provided:
Notifications will be sent to a centralized mailbox
________________________________________
4.2	Existing Resources
All existing resources will be onboarded into the lifecycle system by:
•	Scanning current resources
•	Adding missing lifecycle tags automatically
Special handling:
Resources already in stopped state will be deleted immediately
________________________________________
4.3	Monitoring and Notifications
The system will run daily and evaluate all resources.
Notifications will be sent as follows:
•	3 days before expiration → Warning notification
•	1 day before expiration → Final notification
Notifications will be sent to:
•	OwnerEmail (if available)
•	Central mailbox as fallback
________________________________________
4.4	Quarantine Phase (Controlled Stop)
When a resource reaches its expiration date:
•	The resource will be stopped (not deleted immediately)
•	Lifecycle state is updated to:
•	LifecycleState = Quarantine
•	A grace period of:
•	3 days
will begin
Purpose:
•	Allow recovery
•	Prevent accidental data loss
________________________________________
4.5	Recovery During Quarantine
During the quarantine period:
•	Users can: 
o	Restart the resource
o	Extend the expiration date
If the resource is restarted:
LifecycleState = Active
Deletion process is cancelled
________________________________________
4.6	Final Deletion
If no action is taken during quarantine:
After 3 days:
•	A backup will be created (where applicable)
•	The resource will be permanently deleted
________________________________________
5.	Backup Strategy
Before deletion:
•	Backups will be created for applicable resources (e.g., compute, database)
•	Backups will be retained for a limited period (~30 days)
•	Backups will be automatically cleaned up after retention
________________________________________
6.	Expiration Extension Policy
Users are allowed to extend resource lifetime by updating:
ExpiresOn
However:
Maximum extension allowed = 30 additional days
This ensures no resource remains active indefinitely.
________________________________________
7.	Optional Cost Optimization - AWS Scheduler (Optional) 
An optional scheduling feature may be implemented to reduce costs:
•	Stop resources during off-hours (e.g., 11 PM)
•	Start resources during working hours (e.g., 6 AM)
This applies only to:
LifecycleState = Active
Quarantined resources are excluded from scheduling.
________________________________________
8.	Safety Controls
The system includes the following safeguards:
•	Stop before delete (quarantine phase)
•	Grace period before deletion
•	Backup before deletion
•	Ability to recover during quarantine
•	Notification alerts prior to any action
________________________________________
9.	Governance Approach
•	Lifecycle management is fully automated
•	No strict blocking of resource creation (auto-tagging approach)
•	Critical resources may be excluded through governance controls
________________________________________
10.	Expected Benefits
Operational Benefits
•	Eliminates manual cleanup effort
•	Ensure consistent lifecycle management
•	Reduces administrative overhead
________________________________________
Financial Benefits
•	Reduces cloud cost by removing unused resources
•	Prevents long-running idle workloads
•	Optimizes usage patterns
________________________________________
Risk Reduction
•	Prevents accidental deletion through quarantine
•	Ensures recoverability via backups
•	Provides visibility through notifications
________________________________________
11.	Implementation Plan (High-Level)
1.	Implement automated tagging for all resources
2.	Onboard existing resources into lifecycle system
3.	Deploy lifecycle monitoring and notification system
4.	Enable quarantine and deletion process
5.	Introduce optional scheduling for cost optimization
6.	Define rollout strategy prior to production use
________________________________________
12.	Conclusion
This proposal introduces a fully automated, controlled, and safe lifecycle management system that ensures:
•	All resources are temporary and governed
•	Cleanup happens without manual intervention
•	Cost and resource usage are continuously optimized
________________________________________
Final Statement
This solution will transform the sandbox environment into:
A self-maintaining, cost-controlled, and governance-driven cloud environment







