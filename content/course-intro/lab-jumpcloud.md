# Lab: Multi-Cloud SSO

You are welcome to follow the instructor in case you don't have your own AWS account

1. Open browser to [https://jumpcloud.com/](https://jumpcloud.com/)
2. Select `Get Started`
3. Register a new account, email confirmation maybe required
4. Sign-in and proceed to `applications` link
5. Type: `aws` and select `amazon web services` and then `configure` link
6. Open another browser tab/window and proceed to Aws -&gt; Services -&gt; IAM
7. We will need Aws `account number` shown under `AWS account ID:` label
8. JumpCloud Console
   1. For display label type: `Aws Read-Only`
   2. Find the attribute `https://aws.amazon.com/SAML/Attributes/Role`
   3. In the value for this attribute replace `YOUR_AWS_ACCOUNT_NUMBER` with account number copied from Aws console
   4. There is another token we will need to replace later: `ROLE_1`
   5. Select `activate` link and `save`
   6. From the list select the newly configured application
   7. Select ``export metadata` line and save the file``
9. Aws Console
   1. Proceed to Services -&gt; IAM
   2. Select `Identity Providers`
   3. Add new provider of type `SAML`
   4. Type provider name: `JumpCloud`
   5. And upload the metadata file downloaded from JumpCloud console
   6. 



