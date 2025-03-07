# aws-storage-security

```mermaid
---
title: Storage Backup
---
flowchart LR
    subgraph server[On-premise]
    app[Application Server]
    volumeGateway(AWS Volume Gateway)
    app <-->|iSCSI| volumeGateway 
    end
    subgraph aws[AWS]
    subgraph s3[S3]
    volumeStorage[(Volume Storage)]
    snapshot[Snapshot]
    vaultLock{Vault Lock}
    volumeStorage --> snapshot
    snapshot --> vaultLock
    end
    subgraph ec2[Backup EC2]
    ec2Instance[EC2 Instance]
    ec2Instance <--> backupVolume[(Backup Volume)]
    createVM[Create EC2 Instance with Backup Volume]
    end
    end
    
    volumeGateway --> volumeStorage 
    volumeStorage --> backupVolume
    createVM --> ec2Instance