# AWS Backup and Restore – Task 7

## Cloud Computing Internship

This project demonstrates how to configure automated backups for an Amazon EBS volume using AWS Backup, perform a real restore into a new EBS volume, and verify that deleted data can be recovered successfully.

---

## Objective

To configure an automated backup schedule for an EBS volume, test the backup by intentionally deleting data, restore the volume from the recovery point, and verify that the deleted data is recovered.

---

## AWS Services Used

- Amazon EC2
- Amazon EBS
- AWS Backup
- IAM

---

## Architecture

```text
EC2 Instance
     |
     | attached
     v
Original EBS Volume
     |
     | AWS Backup
     v
AWS Backup Vault
     |
     | Recovery Point
     v
Restored EBS Volume
     |
     | attached to EC2
     v
Recovered Data

```
## Resources
**EC2 Test Instance**
- Name: veda-task7-backup-test
- Instance type: t3.micro
- Region: Mumbai (ap-south-1)
- Availability Zone: ap-south-1b
  
**Original EBS Volume**
- Name: veda-task7-backup-volume
- Type: gp3
- Size: 1 GiB
- Availability Zone: ap-south-1b
  
**AWS Backup Plan**
- Plan name: veda-task7-backup-plan
- Rule name: veda-task7-daily-backup
- Backup vault: Default
- Schedule: Daily at 18:30 IST
- Time zone: Asia/Calcutta
- Retention: 7 days
- Cold storage: Disabled

  
## Backup Configuration
The backup plan was configured with:
- Daily automated backup schedule configured
- 8-hour backup start window
- 7-day completion window
- 7-day retention period
- Default AWS Backup vault
- EBS volume as the protected resource
The deployment configuration is available in:
backup-plan.json


## Backup Test Data
A test file was created on the original EBS volume:
VEDA TASK 7 - AWS BACKUP RESTORE TEST

The file was verified before performing the restore test.


## Backup Execution
An on-demand backup was also created to obtain an immediate recovery point for testing.
**Backup Job**
- Backup job ID: 875c5855-63f6-4c25-abb0-31afe8e8916a
- Resource type: EBS
- Backup status: Completed
- Backup type: Snapshot
- Retention: 7 days

  
## Real Restore Test
To prove that the backup was usable, the test data was intentionally deleted from the original EBS volume.
**The original file:** backup-proof.txt

**was deleted from:** /backup-test/

The directory was then verified to ensure that the file was no longer present.
The completed AWS Backup recovery point was restored into a new EBS volume.


## Restore Details
**Recovery Point**
- Recovery point: snapshot/snap-0a47de935b3a2cd8a
- Status: Completed
- Backup type: Snapshot
  
**Restore Job**
- Restore job ID: 26142c50-e12c-45c3-81db-6594f7bd46a1
- Resource type: EBS
- Restore status: Completed
- Restore duration: 1 minute
  
**Restored Volume**
The recovery point was restored into a new 1 GiB gp3 EBS volume in:
ap-south-1b

The restored volume was attached to the EC2 test instance and mounted separately.


## Recovery Verification
**The restored volume was mounted at:** /restore-test

**The previously deleted file was found:**  /restore-test/backup-proof.txt

**Its contents were:** VEDA TASK 7 - AWS BACKUP RESTORE TEST

This confirms that the deleted data was successfully recovered from the AWS Backup recovery point.


## Retention Policy
A 7-day retention period was selected.
**Reason**
Seven days provides a reasonable recovery window for a temporary internship/test environment while avoiding unnecessary long-term backup storage costs.
For production systems, retention should be based on:
- Business requirements
- Recovery Point Objectives (RPO)
- Recovery Time Objectives (RTO)
- Compliance requirements
- Storage cost
- Data criticality


## Why an Untested Backup Is Not Reliable
A backup existing in a storage location does not guarantee that it can actually be restored.
A backup should be tested because:
- The recovery point may be corrupted.
- Required permissions may be missing.
- The restore process may fail.
- Restored data may be incomplete.
- Configuration or compatibility issues may prevent recovery.
In this task, a real restore was performed and the recovered file was verified.


## Snapshot vs Full Backup

**Snapshot**

A snapshot is a point-in-time copy of a storage resource. Amazon EBS snapshots use incremental storage after the initial snapshot, meaning only changed blocks need to be stored for subsequent snapshots.

**Full Backup**

A full backup represents a complete backup of the selected data or resource according to the backup system being used. Depending on the technology, full backups can require more storage and processing time.

AWS Backup provides centralized backup management, scheduling, retention policies, and recovery point management across supported AWS resources.


## Backup Retention Decision
Backup retention should be selected according to:
1. Business recovery requirements
2. Regulatory/compliance requirements
3. Data change frequency
4. Recovery objectives
5. Storage costs
For this temporary test environment, 7 days was sufficient.


## Evidence
|               Evidence               |              Description                 |
|--------------------------------------|------------------------------------------| 
| `01-task7-ec2-instance-running`      | EC2 test environment                     |
| `02-task7-backup-volume-created`     | Original EBS volume                      |
| `03-task7-ssh-connected`             | EC2 SSH connection                       |
| `04a-task7-backup-plan-name`         | Backup plan configuration                |
| `04b-task7-backup-schedule`          | Automated schedule and retention         |
| `05-task7-backup-resource-selection` | EBS resource assignment                  |
| `06-task7-backup-job-completed`      | Successful backup job                    |
| `07-task7-data-deleted`              | Intentional deletion of test data        |
| `08-task7-recovery-point`            | Completed recovery point                 |
| `09-task7-restore-job-completed`     | Successful restore and 1-minute duration |
| `10-task7-restored-volume-created`   | New volume created from recovery point   |
| `10-task7-restored-volume-attached`  | Restored volume attached to EC2          |
| `11-task7-restored-data-match`       | Recovered file and matching contents     |
| `12-task7-backup-plan-json`          | Backup deployment configuration          |


## Cleanup
After completing and verifying the restore test, temporary AWS resources were cleaned up:
- EC2 test instance terminated
- Original test EBS volume deleted
- Restored EBS volume deleted
- AWS Backup recovery point deleted
- Backup resource assignment deleted
- AWS Backup plan deleted
- Temporary security group deleted


## Conclusion

This task demonstrated the complete backup and recovery lifecycle using AWS Backup and Amazon EBS:

**Automated Backup Schedule → Recovery Point → Data Deletion → Restore → Data Verification → Cleanup**

The backup schedule was configured with a daily frequency and 7-day retention. An on-demand backup was used to obtain an immediate recovery point for the restore test.

The restore operation completed in **1 minute**, and the intentionally deleted `backup-proof.txt` file was successfully recovered with its original contents intact.

This demonstrates that the backup was not only configured successfully but also **validated through an actual restore test**.
