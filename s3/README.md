_SDK Task_
1. Create S3 Bucket
2. List S3 Buckets
3. Delete S3 Buckets
4. Deleting non-empty S3 Bucket
5. Upload file to S3 Bucket
6. Upload a file to S3 Bucket
7. Upload multiple files to S3 bucket
8. Upload generated file object data to S3 Bucket
9. Get a list of files from S3 Bucket
10. Filtering results of S3 list operation
11. Download file from S3 Bucket
12. Read files from the S3 bucket into memory
13. Delete S3 objects
14. Rename S3 file object
15. Copy file objects between S3 buckets
16. Create S3 Bucket Policy
17. Delete S3 Bucket Policy
18. Generate S3 presigned URL
19. Enable S3 Bucket versioning


*CNF Task*

*Introduction to S3 with awscli*
## Create a Bucket
S3 buckets are located in regions, but their names are globally unique. Using "aws s3", create a bucket: 
   * Use the us-west-2 region.
   * Call the bucket "ule-your-AWS-username".
   * List the contents of the bucket.

## Upload Objects to a Bucket
Add an object to your bucket:
   * Create a local subdirectory, "data", for s3 files and put a few files in it.
   * Copy the file to your bucket using the "aws s3" command. Find more than one way to upload it.
   * List the contents of the bucket after each upload.

## Exclude Private Objects When Uploading to a Bucket
Add a private file to your data directory. Then, upload the directory to your bucket again without including the private file.
   * Verify after uploading that the file doesn't exist in the bucket.
   * Did you find two different ways to accomplish this task? If not, make sure to read the documentation on sync flags.

*Intro to S3 Permissions*
## Recreate the Bucket with Public Data
Create your bucket again and upload the contents of your "data" directory with the "aws s3 sync" command.
   * Include the "private.txt" file this time.
   * Use a "sync" command parameter to make all the files in the bucket publicly readable.

## Use the CLI to Restrict Access to Private Data
   * You just made "private.txt" publicly readable. Ensure that only the bucket owner can read or write that file without changing the permissions of the other files.

## Using the API from the CL
The aws s3api command gives you a lot more options. Remove the bucket again, then recreate it to start fresh.
Make all files publicly readable, grant yourself access to do anything to all files, and block access to "private.txt" unless you're an authenticated user:
   * Create and assign an IAM policy to explicitly grant yourself maintenance access.
   * Set a bucket policy to grant public read access.
   * Set an S3 ACL on "private.txt" to block read access unless you're authenticated.
When you're done, verify that anybody (e.g. you, unauthenticated) can read most files but can't read "private.txt", and only you can modify file and read "private.txt".

*S3 Versioning and Lifecycle Policies*
See Object Versioning, Lifecycle Management, and Storage Class Analysis in the S3 docs for information on the labs below. You'll use both the CLI & CloudFormation. In a professional capacity, you're much more likely to write policies and bucket options with CloudFormation, and they're easier to write in its templating language, but some of these tasks -- like those managing data -- require the CLI.

## Set up for Versioning
Experiment with bucket versioning. Using the CLI, delete the bucket stack you created for Principle 2, so that we can start out fresh. Make a copy of the stack template you just wrote above, and modify it for this lab:
   * Create the stack with versioning enabled on the bucket
   * Upload your local data files to the bucket with the CLI.
   * Modify your local data files, and sync those files to the bucket again.
   * Inspect your bucket's objects after syncing and see how many versions there are.
   * Fetch the original version of an object.

## Object Versions
   * Delete one of the objects that you changed.

_Question: Deleted Object Versions_
Can you still retrieve old versions of the object you removed?

_Question: Deleting All Versions_
How would you delete all versions?

## Tagging S3 Resources
Tag one or more of your objects or buckets using "aws s3api", or add tags to your bucket through CloudFormation. View the tags on them through the CLI or the console.

_Question: Deleting Tags_
Can you change a single tag on a bucket or object, or do you have to change all its tags at once?
(See aws:cloudformation:stack-id and other AWS-managed tags.)

## Object Lifecycles
Create a lifecycle policy for the bucket:
   * Move objects to the Infrequent Access class after 30 days.
   * Move them to Glacier after 90 days.
   * Expire all noncurrent object versions after 7 days.
   * Remove all aborted multipart uploads after 1 day.
After updating your stack, use the S3 console's Management Lifecycle tab to double-check your settings.

*S3 Object Encryption*
## Server-Side Encryption
Modify your bucket policy to require server-side encryption with an S3-managed key ("SSE-S3").

_Question: Encrypting Existing Objects_
Do you need to re-upload all your files to get them encrypted?

## SSE with KMS Keys
Change your bucket policy to require KMS encryption for all objects.
   * Update your stack.
   * Use the S3 API to identify the key used to encrypt one of the files in your bucket.
   * Modify the local copy of that file and re-sync your bucket
   * Let S3 generate a CMK for you.
   * Use the S3 API again to check the key ID.

The 1st key should be the S3-managed AES key. The 2nd should be your KMS key.

_Question: KMS vs S3 Managed Keys_
Look through the S3 encryption docs. What benefits might you gain by using a KMS key instead of an S3-managed key?

_Question: Customer Managed CMK_
Going further, what benefits might you gain by using a KMS key you created yourself?

## Using Your Own KMS Key
Use your own KMS key to encrypt files in S3.
   * Generate a new KMS key by adding an AWS::KMS::Key to your stack or using the "aws kms create-key" command.
   * Assign an alias to the key as well.
   * Re-upload some of your test files using that key.
   * Again use the S3 API to verify the key ID used on objects before and after the change.

_Question: CMK Alias_
Can you use the alias when uploading files?





