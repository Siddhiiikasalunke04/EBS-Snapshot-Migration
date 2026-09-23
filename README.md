# AWS Cross-Region EBS Snapshot Migration Project



## Introduction

This project demonstrates how to migrate data securely between AWS Regions using Amazon Elastic Block Store (EBS) Snapshots.
In this project, a Linux EC2 instance was launched in the Virginia (us-east-1) region. A new 3 GB EBS volume was created, attached to the instance, partitioned, formatted with the EXT4 file system, mounted, and used to store the project data.
An EBS Snapshot of the volume was then created and copied to the Mumbai (ap-south-1) region. From the copied snapshot, a new EBS volume was created, attached to a new EC2 instance, and the project data was successfully verified.



## Architecture Diagram

![](./images/architecture%20diagram.jpeg)

### Architecture Overview 

1. **Source Environment (US East - N. Virginia - `us-east-1`)**:
   * An EC2 instance is deployed inside a VPC.
   * An additional EBS volume (3 GB) is attached to the instance, partitioned, formatted with `ext4`, and mounted to `/data` containing project files.
   * An EBS Snapshot is taken to capture point-in-time block data.
2. **Cross-Region Replication**:
   * The snapshot is replicated asynchronously across AWS regions from `us-east-1` to `ap-south-1` over the AWS global network backbone.
3. **Destination Environment (Asia Pacific - Mumbai - `ap-south-1`)**:
   * A new EBS volume is created directly from the replicated snapshot in the same Availability Zone as the target instance.
   * The volume is attached to the Mumbai EC2 instance, mounted to `/data`, and verified for data integrity.



## Project Workflow

### Step 1 – Launch Source EC2 Instance

Launch an Amazon Linux EC2 instance in the Virginia Region.


### Step 2 – Create a New EBS Volume

Create a 3 GB gp3 EBS Volume in the same Availability Zone.

![](./images/virginia%20volume%20create.png)


### Step 3 – Attach EBS Volume

Attach the newly created volume to the EC2 instance.

![](./images/virginia%20attach%20volume.png)


### Step 4 – Verify Attached Storage

Verify that the new device (/dev/nvme1n1) is attached.

![](./images/virginia%20lsblk%20after%20volume.png)


### Step 5 – Create a New Partition

Create a primary partition using the default values.

![](./images/virginia%20partition%20create%201.png)
![](./images/virginia%20partition%20create%202.png)


### Step 6 – Create File System

Format the partition using the EXT4 file system.

![](./images/virginia%20create%20file%20system.png)


### Step 7 – Create Mount Directory, Mount the File System, and Copy Project Data

Create a mount directory, mount the formatted EBS partition to the directory, and copy the existing project data to the mounted EBS volume. Finally, verify that the data has been copied successfully.

![](./images/virginia%20mount%20directory%201.png)
![](./images/virginia%20mount%20directory%202.png)
![](./images/virginia%20mount%20directory%203.png)


### Step 8 – Create an EBS Snapshot

- Select the 3 GB EBS Volume.
- Create a Snapshot.

![](./images/create%20snapshot%201.png)
![](./images/create%20snapshot%202.png)


### Step 9 – Copy Snapshot to Mumbai Region

Copy the Snapshot from Virginia to Mumbai.

![](./images/copy%20snapshot%201.png)
![](./images/copy%20snapshot%202.png)
![](./images/copy%20snapshot.png)


### Step 10 – Create a New Volume from Snapshot

- Switch to the Mumbai Region.
- Create a new EBS Volume from the copied Snapshot.

![](./images/create%20volume%20from%20snapshot%201.png)
![](./images/create%20volume%20from%20snapshot%202.png)
![](./images/create%20volume%20from%20snapshot%203.png)
![](./images/create%20volume%20from%20snapshot.png)


### Step 11 – Attach Volume to Mumbai EC2

- Launch the TCS EC2 Instance.
- Attach the newly created EBS Volume.

![](./images/mumbai%20attach%20volume.png)


### Step 12 – Mount the Volume and verify this 

![](./images/mumbai%20mount%20volume.jpeg)

The project directory should be visible.

![](./images/project%20successfully%20visible.png)



## Project Summary

This project demonstrates the complete process of AWS Cross-Region EBS Snapshot Migration. A 3 GB Amazon EBS volume was created, partitioned, formatted with the EXT4 file system, mounted, and used to store project data. An EBS Snapshot was created and copied from the Virginia Region (us-east-1) to the Mumbai Region (ap-south-1). A new EBS volume was then created from the copied snapshot, attached to a Mumbai EC2 instance, and mounted successfully. Finally, the project data was verified, proving successful cross-region migration and data recovery using Amazon EBS Snapshots.