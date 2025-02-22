 ---
![[home.png]]

---


### Hosting

What is Amazon S3

Amazon S3 (Simple Storage Service) is a scalable, secure, and highly available cloud storage service offered by AWS. It allows users to store, retrieve, and manage any amount of data from anywhere on the internet.

#### How I used Amazon S3 in this project.

I used Amazon S3 in today's project to host a static website. Here's what I did: 

 Created an S3 Bucket  Named it web-site-project-chandra-kant and ensured the
name was globally unique.

 Uploaded Website Files - Added index.html and the website assets folder.

 Enabled Static Website Hosting - Configured the bucket settings to serve content via the S3 website endpoint:

URL:  http://web-site-project-chandra-kant.s3-website.ap-south-1.amazonaws.com 

 Fixed Access Issues - Resolved a connection error by: Making files public using ACLs.
Updating the Bucket Policy to allow public read access.  

 Tested Website - Visited the S3 bucket endpoint to confirm everything was working.

### This project took me around 15-20 minutes to complete.

Breakdown of Time Spent:  
✅  Creating the S3 Bucket & Uploading Files - 5 minutes  
✅  Enabling Static Website Hosting - 2 minutes  
✅  Fixing Access Issues - 403 Error, ACLs, Bucket Policy) - 7 minutes
✅  Testing & Verifying Website Accessibility - 3-5 minutes.  
#### How I Set Up an S3 Bucket

Creating an S3 bucket took me just a few seconds. The process is almost instantaneous, as AWS provisions the bucket immediately after you specify a  unique name, select a region, and configure optional settings like versioning, encryption, or ACLs.


When we say Amazon S3 - Simple Storage Service) buckets have to be globally unique, it means that the name of an S3 bucket must be unique across all AWS accounts and regions worldwide.

![[s3-bucket.png]]


#### Upload Website Files to S3

index.html and image assets 
I uploaded two files to my S3 bucket – they were

index.html (a simple HTML file) and 
NextWork -  Everyone should be in a job they love_files is a directory containing the supporting assets. Specifically, index.html likely contains the HTML structure.

![[object.png]]

#### Static Website Hosting on S3

Website hosting means storing website files - HTML, CSS, JavaScript, images)  
on a server that is accessible via the internet so that users can visit the site from their browsers.

To enable website hosting with my S3 bucket, I enabled Static Website Hosting,  
set index.html as the default file, uploaded files, updated ACLs and bucket policy for public access, and tested the site via the S3 endpoint URL.

An ACL  - (Access Control List) is a set of rules that defines which AWS accounts  
or IAM users have access to an S3 bucket or object and what actions they can  
perform (e.g., read, write). By default, S3 ACLs are disabled.

![[enable-static.png]]

Bucket Endpoints

Once static website hosting is enabled, S3 produces a bucket endpoint URL,  
which is:

http://web-site-project-chandra-kant.s3-website.ap-south-1.amazonaws.com


When I first visited the bucket endpoint URL, I saw a 403 Forbidden error or a  similar access-related message. The reason for this error was that S3 bucket permissions were not set correctly for public access. By default, S3 blocks public access to

![[403.png]]

#### To resolve this connection error,

I selected the checkboxes next to my index.html file and the folder containing website assets.

Then, in the Actions dropdown, I chose "Make public using ACL"

![[home.png]]

---
---

## Let's Do some Advance Stuff.........

## Securely Sharing an object using a presigned url.

Upload your secret document to s3.

![[secret-upload-1.png]]

1. **Select the Object**:
    
    - Click on the checkbox next to `secret.pdf` to select it.
    
2. **Access the Actions Menu**:
    
    - With the object selected, click on the "Actions" button located above the list of objects.
    
3. **Choose "Share with a Presigned URL"**:
    
    - From the dropdown menu, select "Share with a presigned URL."
    ![[actions-presigned-url-2.png]]
4. **Set Expiration**:
    
    - You will be prompted to set an expiration time for the presigned URL. Choose an appropriate duration based on how long you want the URL to be valid.
    ![[presigned-time-3.png]]
5. **Generate URL**:
    
    - Click on "Generate URL" or a similar button to create the presigned URL.
    ![[presigned-created-copied-4.png]]

### Step 4: Share the Presigned URL

1. **Copy the URL**:
    
    - Once the presigned URL is generated, copy it to your clipboard.
2. **Share Securely**:
    
    - Share the presigned URL with the intended recipients via a secure method, such as encrypted email or a secure messaging platform.

### Step 5: Access the Object

1. **Open the URL**:
    - Recipients can paste the presigned URL into their web browser to access the object. They will have access until the URL expires.

### Security Considerations

- **Expiration**: Always set an appropriate expiration time to limit access.
- **Permissions**: Ensure that only authorized users have access to generate presigned URLs.
- **HTTPS**: Use HTTPS to encrypt the data in transit.

---
---

## Secure Your Bucket........

Creating a bucket policy to prevent anyone from deleting the bucket or its contents is an important step in securing your AWS S3 resources. Below is a step-by-step guide to creating such a policy:

### Implementation Steps

1. **Access Your S3 Bucket**:
    
    - Log in to the AWS Management Console and navigate to the S3 service.
    - Select the bucket `web-site-project-chandra-kant`.
2. **Edit Bucket Policy**:
    
    - Go to the "Permissions" tab.
    - Scroll down to the "Bucket policy" section and click "Edit".
    ![[permission-tab-edit-1.png]]
3. **Apply the Policy**:
    
    - Copy and paste the above JSON policy into the policy editor.
    - Click "Save changes" to apply the policy.
    ![[edit-policy-save-changes-2.png]]

```json

{
"Version": "2012-10-17",
"Id": "MyBucketPolicy",
"Statement": [{
  "Sid": "BucketPutDelete",
  "Principal": "*",
  "Effect": "Deny",
  "Action": "s3:DeleteObject",
  "Resource": "arn:aws:s3:::web-site-project-chandra-kant/index.html"}]
}
```
### Explanation

- **Version**: Specifies the version of the policy language. Here, it is `2012-10-17`.
    
- **Id**: An identifier for the policy, in this case, `MyBucketPolicy`.
    
- **Statement**: Contains the individual permissions within the policy.
    
    - **Sid**: A unique identifier for the statement, here named `BucketPutDelete`.
        
    - **Principal**: Specifies who the statement covers. `"*"` means the statement applies to all users.
        
    - **Effect**: Specifies whether the statement allows or denies access. Here, it is set to `Deny`.
        
    - **Action**: Specifies the actions to which the statement applies. In this case, `s3:DeleteObject` is specified, meaning the policy applies to delete actions.
        
    - **Resource**: Specifies the object to which the statement applies. The resource is `arn:aws:s3:::web-site-project-chandra-kant/index.html`, which refers to the `index.html` object in the specified bucket.

#### When you try to delete the s3 it won't..
![[delete-denied-3.png]]
### Security Considerations

- **Scope**: This policy specifically targets the `index.html` object. Ensure that this is the intended object to protect.
- **Principal**: The policy applies to all users (`"*"`). If you want to restrict this to specific users or roles, modify the `Principal` field accordingly.
- **Testing**: After applying the policy, attempt to delete the `index.html` object to ensure the policy is functioning as expected.