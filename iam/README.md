Identity and Access Management (IAM) is the authentication and authorization service used to control access to virtually everything in AWS.

*IAM policies*
To define a level of access to one or another AWS service, you need to define an IAM policy. A policy is a JSON document that contains a set of rules defined in one or more statements. Each policy grants a specific set of permissions to AWS APIs, and it can be attached to one or more IAM identities (user, group, and role).

There are two types of policies:

1. Managed policies can be created and attached to multiple entities. AWS has several built-in managed policies that cover a wide variety of use cases. You can assign many managed policies to IAM users, roles or groups. You can also create your own managed policies according to your needs.
2. Inline policies are directly applied to IAM entities (users or roles), and don’t have distinctive ARNs. Those policies can not be reused.

When working with IAM, you need to understand its terminology:

Identities are IAM resources that define users, groups, and roles. You can attach a policy to an IAM identity.
1. Entities are objects that AWS uses for authentication, for example, IAM users or roles
2. Principals are people or applications that uses IAM users or roles in AWS account to make any actions or API calls
3. IAM policy allows or denies a set of actions to one or more principals on certain resources.

There are two main categories of IAM policies:
1. Identity-based policies answer the question “Which API calls can this identity make to which resources?”
2. Resource-based policies define “Which identities can perform which actions with me?”

_SDK Task_
1. Create IAM policy
2. List IAM policies
3. Filter IAM policies
4. Describe IAM policy
5. Delete IAM policy
6. Attach IAM policy to a role
7. Detach IAM policy from the role

*IAM roles*
The IAM role contains a set of permissions an IAM entity can perform and what other IAM entities (users or roles) can assume that role. A role is not directly linked to a person or a service. Rather it can be assumed by any resource that the role grants permission to. Role credentials are always temporary and rotated periodically by the AWS Session Token Service(STS).

Instead of assigning permissions to an entity directly, roles allow an entity to be granted permissions temporarily (on an as-needed basis) to perform tasks. This enforces the least privilege principle, which is based on both identity and time, as you can restrict entities to both the minimum amount of access needed and the minimum amount of time needed to complete a task.

A role is both a principal and identity in AWS and has the primary purpose of granting temporary permissions to perform API calls in an account. To use a role, it has to be assumed.

Each role has trust relationships that determine the entities that can assume the role. It also has a set of permissions that define which privileges entities get after they assume the role.


_SDK Task_
1. Create IAM role
2. List IAM roles
3. Filter IAM roles
4. Describe IAM role
5. Delete IAM role


*IAM groups*
An IAM group is a collection of users and permissions assigned to those users. Groups provide a convenient way to manage users with similar needs by categorizing them to their requirements. Then, you can manage permissions for all those users at once through the group.

_SDK Task_
1. Create IAM group
2. List IAM groups
3. Filter IAM groups
4. Describe IAM groups
5. Delete IAM group

*IAM instance profiles*
You can provide your EC2 instances required permissions to access AWS resources without using API Access Keys by assigning them Instance Profiles. Instance Profile provides a mechanism to assign an IAM role to an instance.


_SDK Task_
1. Create IAM instance profile
2. Add role to IAM instance profile
3. List IAM instance profiles
4. Filter IAM instance profiles
5. Describe IAM instance profile
6. Delete IAM instance profile
7. Attach IAM instance profile to the EC2 instance
8. Detach IAM instance profile from the EC2 instance

*IAM users*
An IAM user is an entity you create in AWS to represent the person or application that can interact with AWS services.

_SDK Task_
1. Create IAM user
2. List IAM users
3. Filter IAM users
4. Describe IAM user
5. Delete IAM user
6. Add IAM user to group
7. Remove IAM user from group
8. Attach IAM policy to the user
9. Detach IAM policy from the user
10. Change IAM user password
11. Create IAM Access Keys
12. List IAM access keys
13. Delete IAM access keys
14. Create login profile for IAM user
15. Delete login profile from IAM user


_CNF Task_
##  IAM Role
Create a CFN template that specifies an IAM Role.
   * Provide values only for required attributes.
   * Using inline Policies, give the Role read-only access to all IAM resources.
   * Create the Stack.
   * Use the awscli to query the IAM service twice:
       * List all the Roles
       * Describe the specific Role your Stack created.

## Customer Managed Policy
Update the template and the corresponding Stack to make the IAM Role's inline policy more generally usable:
   * Convert the IAM Role's inline Policies array to a separate customer managed policy resource.
   * Attach the new resource to the IAM Role.
   * Update the Stack using the modified template.

## Customer Managed Policy Re-Use
Update the template further to demonstrate reuse of the customer managed policy:
   * Add another IAM Role.
   * Attach the customer managed policy resource to the new role.
   * Be sure that you're not referencing an AWS managed policy in the role.
   * Add/Update the Description of the customer managed policy to indicate the re-use of the policy.
   * Update the Stack. Did the stack update work?
       * Query the stack to determine its state.
       * If the stack update was not successful, troubleshoot and determine why.

## AWS-Managed Policies
Replace the customer managed policy with AWS managed policies.
   * To both roles, replace the customer managed policy reference with the corresponding AWS managed policy granting Read permissions to the IAM service.
   * To the second role, add an additional AWS managed policy to grant Read permissions to the EC2 service.
   * Update the stack.

## Trust Policy
Create a CFN template that creates an IAM Role and makes it possible for your User to assume that role.
   * The role should reference the AWS managed policy ReadOnlyAccess.
   * Add a trust relationship to the role that enables your specific IAM user to assume that role.
   * Create the stack.
   * Using the AWS CLI, assume that new role. If this fails, take note of the error you receive, diagnose the issue and fix it.

## Explore the assumed role
   * Using the AWS CLI, assume that updated role and list the S3 buckets in the us-east-1 region.
   * Acting as this role, try to create an S3 bucket using the AWS CLI.
       * Did it succeed? It should not have!
       * If it succeeded, troubleshoot how Read access allowed the role to create a bucket.

## Add privileges to the role
Update the CFN template to give this role the ability to upload to S3 buckets.
   * Create an S3 bucket.
   * Using either an inline policy or an AWS managed policy, provide the role with S3 full access
   * Update the stack.
   * Assuming this role again, try to upload a text file to the bucket.
   * If it failed, troubleshoot the error iteratively until the role is able to upload a file to the bucket.

## Unrestricted access to a service
Create a CFN template that generates two S3 buckets and a Role, and demonstrate you have full access to each bucket with this new role.
   * Code a Role your User can assume with a customer managed policy that allows full access to the S3 service.
   * Create the stack.
   * As your User:
   * list the contents of your 2 new buckets
       * upload a file to each new bucket
       * Assume the new role and repeat those two checks as that role.

## Resource restrictions
Add a resource restriction to the role's policy that limits full access to the S3 service for just one of the two buckets and allows only read-only access to the other.
   * Update the stack.
   * Assume the new role and perform these steps as that role:
       * List the contents of your 2 new buckets.
       * Upload a file to each new bucket.
  # Were there any errors? If so, take note of them.
  # What were the results you expected, based on the role's policy?

## Conditional restrictions
   * Update the stack.
   * Assume the new role and perform the remaining directives as that role.
   * Try to list a file in the root of the available bucket
      * If it worked, fix your policy and update the stack until this fails.
   * Try to list that same file but now with the proper object key prefix.
      * If it doesn't work, troubleshoot why and fix either the role's policy or the list command syntax until you are able to list a file.