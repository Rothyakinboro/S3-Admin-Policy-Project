# AWS S3 Admin Policy Project

## Description

This project demonstrates how to create and manage an AWS IAM group with a custom policy that allows specific actions on S3 buckets in a development environment, while restricting those actions in a production environment. The steps include creating S3 buckets, defining an IAM policy, assigning the policy to a group, adding a user to the group, and testing the policy's functionality.

## Steps

### 1. Create S3 Buckets in Dev and Prod

- **Description**: Two S3 buckets are created: one for the development environment (`wd102-33-dev`) and another for the production environment (`wd102-33-prd`).
- **Reason**: Separate S3 buckets for different environments help in segregating data and permissions, ensuring that production data remains secure.

### 2. Create a Policy to Manage S3 Permissions

- **Description**: A custom IAM policy is created that allows users to put and delete objects in the development bucket (`wd102-33-dev`) but denies these actions in the production bucket (`wd102-33-prd`).
- **Policy**:
    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "AllowListAndGetAllBuckets",
          "Effect": "Allow",
          "Action": [
            "s3:ListAllMyBuckets",
            "s3:ListBucket",
            "s3:GetObject"
          ],
          "Resource": [
            "arn:aws:s3:::*",
            "arn:aws:s3:::*/*"
          ]
        },
        {
          "Sid": "AllowPutDeleteInDev",
          "Effect": "Allow",
          "Action": [
            "s3:PutObject",
            "s3:DeleteObject"
          ],
          "Resource": "arn:aws:s3:::wd102-33-dev/*"
        },
        {
          "Sid": "DenyPutDeleteInPrd",
          "Effect": "Deny",
          "Action": [
            "s3:PutObject",
            "s3:DeleteObject"
          ],
          "Resource": "arn:aws:s3:::wd102-33-prd/*"
        }
      ]
    }
    ```
- **Reason**: The policy is designed to allow users to freely work in the development environment while protecting the integrity of the production environment by restricting object manipulation.

### 3. Create an S3Admin Group and Assign the Policy

- **Description**: An IAM group named `s3Admins` is created, and the custom policy is attached to this group.
- **Reason**: Grouping users with similar permissions makes it easier to manage and scale access control across multiple users.

### 4. Create an S3Admin User with Console Access

- **Description**: A new IAM user with the name `s3Admin` is created. This user is granted console access and is added to the `s3Admins` group.
- **Reason**: The user needs access to the AWS Management Console to manage and test S3 resources as defined by the policy.

### 5. Sign in to the Console as the Newly Created User

- **Description**: The `s3Admin` user signs in to the AWS Management Console.
- **Reason**: To verify the permissions granted by the policy and ensure that the actions can only be performed as intended.

### 6. Test the Policy's Functionality

- **Description**: The `s3Admin` user attempts to put and delete objects in both the development and production buckets.
- **Outcome**: The user is able to manage objects in the development bucket (`wd102-33-dev`) but is denied these actions in the production bucket (`wd102-33-prd`), confirming the policy works as expected.
- **Reason**: Testing is crucial to ensure that the policy is correctly implemented and the security measures are effective.

## Conclusion

This project successfully demonstrates the creation of a secure IAM policy that segregates permissions between development and production environments in AWS S3. By following these steps, similar policies can be implemented to maintain strict access control in any AWS environment.
