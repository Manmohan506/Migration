Prepare your AWS Environment
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

