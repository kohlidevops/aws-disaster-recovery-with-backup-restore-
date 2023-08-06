# aws-disaster-recovery-with-backup-restore

Consider a small E-Commerce website running on EC2 instances and using an DynamoDB table. Regular backups of the website's files and database are taken and stored in Amazon Backup. In case of a disaster, such as data corruption or accidental deletion, you can restore the website by launching new EC2 instances and restoring the database from the backups.

**Method -1: Restore data from data loss and data corrupt**
**Backup and Restore – Point in Time Recovery (PITR), Backup and Cross-Region**

Please configure a basic setup to start a Disaster Recovery using below link

https://github.com/kohlidevops/aws-disaster-recovery/blob/main/README.md

**To enable PITR in DynamoDB Backup**

Navigate to DynamoDB(Mumbai region) and edit the backup to enable PITR.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/27f044f4-101c-4d9a-b33d-ad60835b1a48)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/81feced8-5cac-42e7-b9f7-3643f40eabfb)

**To create a AWS Backup for DynamoBD**

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/e47b5942-8cb3-4391-973e-4914514655ac)

Navigate to AWS Backup console – create a backup plan.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/d673f24c-d49d-453f-bc9e-60f2eff31a29)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/a6ab50ee-04bd-4e17-9bef-d42e18edec9b)

Let’s go ahead to create a backup plan.

Backup plan has been created. Now assigned the DynamoDB resources in Backup plan.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/6f413289-9e6b-432b-8778-e7c76f92549d)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/37814e48-6c32-45cb-8bcd-adb240b4e816)

**To create a On-Demand backup**

To create a On-Demand backup for testing purpose. 

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/bece34d2-b71a-4620-9ef4-aaabc0e91eb7)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/87173607-7701-42dd-86d3-728c4774f65d)

The on-demand backup has been initiated. You can see the process in AWS Backup – Jobs

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/4abba20d-ddac-4fcd-8fa5-bb937b13261d)

After the on-demand backup completion, you can copy this backup from Mumbai to N.Virginia region. I'm going to use Mumbai and N.Virginia regions are Primary and Secondary region.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/bb73731f-878d-45dd-b326-00c080774eb3)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/0f01ddcc-1f3a-430e-a16a-0852968344ec)

Once the backup copy has been completed, you can check with AWS Backup – Protected resources in N.Virginia region

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/a0da4aa6-1e32-4ecf-b36e-e436c2fbf1e4)

**To create some records in DynamoDB through application**

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/6e70ca56-f4fe-4228-9fa8-6ec31d515955)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/6b3aa4be-a09f-4c9e-9731-ea3d108891a5)

Let’s go and check with items in DynamoDB table.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/b5884aff-4a9a-490c-a05b-f3166c2c9d9c)

Let’s add some data’s again in DynamoDB through application.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/f25efcaf-d9b5-4d1e-ab82-79a29455829f)

The DynamoDB table has been updated.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/be43ca99-d11c-418d-94b6-93ae37a44eec)

Let’s think, our DynamoDB has been hacked by someone else and they demand. For this sake I have updated DynamoDB Table records like below and deleted data randomly.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/edd4b18d-8c65-49eb-8452-8c06553fa846)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/c10d7b6d-f3d9-4c24-9be9-a859c29a3afb)

After malicious attack, my application looks horrible

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/9b1e39f5-6e9c-412c-bf67-91d646395776)

**To recover the full DB backup**

Navigate to AWS DynamoDB – Backup – Select your backup - restore (which backup taken through On-Demand)

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/10519b24-1382-4f48-b1c0-ad30a1401fd4)

Name it and initiate the restoration process

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/69ec7d95-d038-4d5e-9f9e-229962b14fde)

After the restoration process, the restored-backup table is available.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/18ec7dab-db31-48be-b099-c1ca286db2f1)

If I select and explore the table items, I can see one value. Because its restored from On-Demand backup, which has been taken initially. That's why there is no updated data.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/a15b29d1-c78a-4e41-b010-355146f74ba2)

So, This backup doesn’t help me. Because recent data’s unavailable in this restored table. Let's we restore the backup with PITR – Point in Time Delivery.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/883cd4b5-d8df-4e8f-89ed-1d3f37ca7323)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/153f577b-0ed5-4052-97a3-635723f0c65a)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/d1ba940e-a76b-4816-b3f9-3c15080fa8c7)

After restored the backup with PITR, I can be able to see all the data.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/46bd7794-91bf-4faf-aa96-74198ab24214)

We just updated this database in Lambda to continue.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/1343d90f-e39e-42b5-b66a-6347064c6144)

Now refresh the application page to see all the data before the corrupted database.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/assets/100069489/61fc8475-1d2b-49e2-b1c0-2cd23c090b72)

That's it.


































