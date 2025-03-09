# aws-storage-security

```mermaid
---
title: Storage Backup
---
flowchart LR
    subgraph onprem[On-premise]
    subgraph server[Server]
    app[Application Server / Database]
    rclone{RClone}
    app <--> rclone 
    end
    end
    subgraph aws[AWS]
    subgraph s3[S3]
    backup[Backup volume]
    
    backup --> snapshots
    subgraph snapshots[Security and history]
    snapshot[Snapshot]
    vaultLock{Vault Lock}
    end
    end
    lambda[AWS Lambda funtion]
    subgraph ec2[Backup EC2]
    ec2Instance[EC2 Instance]
    ec2Instance <--> backupVolume[(Backup Volume)]
    createVM[Create EC2 Instance with Backup Volume]
    end
    end
    
    app <--> |Scheduled ping| lambda
    lambda --> createVM
    rclone <--> backup 
    s3 --> backupVolume
    createVM --> ec2Instance