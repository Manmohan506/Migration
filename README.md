# Cloud Migration
# Prepare your AWS Environment
Before you can use a migration tool to move servers to the cloud, you’ll need to have the permissions to do so. To do this in AWS, you’ll need to setup a user account with programmatic access and a policy attached to that user that allows it to do the migrations. To do this from the AWS console, login to your AWS Account and go the the Identity and Access Management service (IAM).

First, we’ll create a policy so click on the policies menu within the IAM service window. Click the “Create policy” button. Within this screen, you can select the services you want, or better yet, select the JSON tab and paste in the following json policy. NOTE: don’t worry if you don’t understand this, the CloudEndure portal shows you exactly what policy is needed.


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "elasticloadbalancing:*",
        "cloudwatch:*",
        "autoscaling:*",
        "iam:GetUser",
        "iam:PassRole",
        "iam:ListInstanceProfiles",
        "kms:ListKeys",
        "mgh:*"
      ],
      "Resource": "*"
    }
  ]
}



![image](https://user-images.githubusercontent.com/82562508/138051319-bac32244-dd10-416f-afc6-d7332a2e3187.png)


Click the “Review Policy” button. On the next screen give the policy a name and then click the “Create policy” button.

![image](https://user-images.githubusercontent.com/82562508/138051400-821db06f-9abb-4bf6-8719-c54da408d969.png)

Now that the policy is created, it’s time to add an IAM user who has programmatic access to AWS. Go to the IAM console again and this time select “Users” from the menu. Click the “Add user” button. Give the user a name and select the “Programmatic access” check box to allow access to the API. Next, click the “Next: Permissions” button.

![image](https://user-images.githubusercontent.com/82562508/138051496-fd544401-fdb2-41bd-b39a-da7f8fae7912.png)

At this point it’s a good idea to add users to a group and then assign the permissions to the group instead of the user, but that choice is up to you. I clicked the “Create Group” button to add the user to create a group where I added a name. The important piece is to then select the policy you created earlier to assign it to the user or group.

![image](https://user-images.githubusercontent.com/82562508/138051561-7c07aeff-3cd7-4d30-8209-e83446fc9fff.png)

You’ll see the permissions added.

![image](https://user-images.githubusercontent.com/82562508/138051888-9ffa32df-45ec-4b7d-aa08-0ab4377fff01.png)
Click through the wizard to review and complete the permissions piece of the install. Click the “Create user” button.

![image](https://user-images.githubusercontent.com/82562508/138052057-1fad4661-924e-4546-8c49-8074ebd2fd97.png)

On the last screen, be sure to copy the Access key ID and Secret access key. This is the only opportunity to get the secret access key. This also provides access to the environment, so protect it. Don’t post it on your blog or something foolish. 🙂

![image](https://user-images.githubusercontent.com/82562508/138052458-a51436ac-f199-4edf-8185-26c8a2bff3a8.png)

# Setup CloudEndure


When you activate your CloudEndure license you’ll get a SaaS portal to log into. The first thing you’ll do is to create a new project. Do this by clicking the plus symbol.
![image](https://user-images.githubusercontent.com/82562508/138053265-5ec0c990-0032-4468-866f-81d7aceae2ac.png)




Give the project a name and then select which cloud in which you’ll be migrating. Click “Create Project”
![image](https://user-images.githubusercontent.com/82562508/138053436-ef7c142f-850d-408c-b37a-11bb421db010.png)




Once you create the project you’ll see a message warning you that it’s not been setup yet. Click “Continue” to get started with that.
![image](https://user-images.githubusercontent.com/82562508/138053541-9e02d6bc-2dc3-436d-8ef5-438debc91473.png)




The first thing you’ll need to do in the setup window is to add the AWS Access keys which you copied from the AWS Setup section above. Paste them in here and click Save. Also notice that if you weren’t sure about how to setup the AWS permissions, there are links to the policies so you can easily retrieve them from the wizard.
![image](https://user-images.githubusercontent.com/82562508/138053605-4fa5ed2f-0a12-4c15-a4b0-6fd6b9367305.png)



Next, you’ll need to map your replication to your environment. For instance I set my target to be the us-east-1 region of my AWS account. I also selected a subnet and a security group to apply to the instance when it fails over. You can do replication through VPN or Direct Connect, but if you’re OK with it for a test, you can use the public internet like I did. Proxy servers can also be used if you have that requirement. Once you’re done filling in some of your AWS mappings, click “Save Replication Settings” button.

![image](https://user-images.githubusercontent.com/82562508/138053659-0496b3b6-054e-4349-8615-fa7248d27287.png)


You’ll see a message saying that your project is setup but you still need to install the agent on your machine. If you click the “Show Me How” button, you’ll get some more very straight forward examples of how to complete that process.
![image](https://user-images.githubusercontent.com/82562508/138053764-9e19c7ef-556d-40f1-83e5-ca43014ff112.png)



Note here the Agent Installation token and your installers for both Windows and Linux.
![image](https://user-images.githubusercontent.com/82562508/138053821-769209b4-75be-4552-96fb-0aa1ad8fa500.png)




# Replication
Our project is setup, and AWS is ready for us to move servers. But we need to install an agent on the machine before we start replication. This is nice because it means that it will work on physical or virtual machines migrating to the cloud. For this example, I’ve deployed a Windows Server 2016 VM in my vSphere lab. I logged into my VM and copied the installer provided in the “Show Me How” wizard.

Run the installer on your machine and you’ll be prompted with some questions. First, you’ll need to provide the CloudEndure username or token we saw before. Next you’ll need to pick with disks should be replicated. That’s it!

![image](https://user-images.githubusercontent.com/82562508/138053983-99bc602d-b9f5-4ae9-a522-46ac202aaa78.png)


If you go back to the CloudEndure SaaS portal you’ll see your machine is now listed and that it’s replicating.
![image](https://user-images.githubusercontent.com/82562508/138054019-39974778-ed30-459f-bed7-cd57cfcd4d0a.png)



On the main dashboard, you’ll also notice the initial sync is starting on your machine and can get a glimpse of any other issues in your environment. Now you wait until the replication is complete. Depending on your WAN bandwidth, this could take some time. NOTE: At this point an EC2 instance is stood up in your AWS Account that will serve as a replication endpoint. Be aware of this because you will need to pay for this instance as part of your normal cloud costs.

![image](https://user-images.githubusercontent.com/82562508/138054066-da72a4d4-691a-4a82-b098-464723b6704d.png)


 

# Migrate
Once your machine has replicated it’s data, you should see a machine in the “Require Attention” state on the dashboard. Don’t worry, this is a good thing. Before you do an actual failover, you should test it to make sure it’ll work the way you want it to before doing the real migration.
![image](https://user-images.githubusercontent.com/82562508/138054130-15d05aba-260f-4bf3-bf32-06950d587b7d.png)



 

Before you migrate, it might be work looking at the Blueprint. This is where you can specify the size of the instance to use, the networks it’ll be deployed on, whether it has a public IP and so forth. You can update these settings any time as long as you do it BEFORE you do a migration of course.
![image](https://user-images.githubusercontent.com/82562508/138054190-cf971bc5-ce05-41e8-937e-348cec914bb2.png)



When you’re all set, it’s time to run a test failover. Go to your machines tab and click the machine you want to migrate. Click the “Launch Target Instance” button and then select “Test”. You’ll get a warning message about the process where you can click “Next”.
![image](https://user-images.githubusercontent.com/82562508/138054269-73a02274-bca1-4051-91c9-4dca8c1f3bb2.png)



Select the point in time for your test and then select the “Continue with Test” button.

![image](https://user-images.githubusercontent.com/82562508/138054373-39f203b9-7537-46b3-a4eb-12cffc0a5265.png)


You can look at the job progress as it finishes replication and converts the data into your virtual machine. This process spins up a second EC2 instance temporarily to do the conversion.
![image](https://user-images.githubusercontent.com/82562508/138054426-2286e7d0-630d-465e-b50b-d1e1996dbbe4.png)



When the test is finished, you can look in your EC2 console and see 3 instances related to the process.

Replication Instance which should still be running because it’s still handling the replication of your on prem instance.
The converter which will terminate as the job finishes
The migrated instance itself.

![image](https://user-images.githubusercontent.com/82562508/138054555-c410f466-2e25-4caf-a8c2-1a2987804f56.png)

If you look back at the CloudEndure Portal, you’ll notice you have one machine ready now. It’s been tested and ready to go. During this test process, the machine is still running on-prem and undisturbed.


![image](https://user-images.githubusercontent.com/82562508/138054632-0f0e9daa-49ec-4f86-8c40-18d875d667e1.png)

Now, you can destroy your test EC2 instance once you’re done with all of your testing. You’re now still replicating and ready for a failover. When you’re ready to do it, go to machines again and select the machine to failover. Select the “Launch Target Instance” button and this time select Recovery.

![image](https://user-images.githubusercontent.com/82562508/138054761-46c14cb9-6c8e-44c7-87c8-a9e9cba9a432.png)


Repeat the same process you did for the test recovery and wait for it to complete.

 
