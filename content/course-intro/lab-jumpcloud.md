# Lab: Multi-Cloud SSO

## Sample SSO Provider

1. Open browser to [https://jumpcloud.com/](https://jumpcloud.com/)
2. Select `Get Started`
3. Register a new account, an email confirmation may be required

## [AWS Integration](https://support.jumpcloud.com/support/s/article/single-sign-on-sso-with-amazon-aws-in-iam-2019-08-21-10-36-47)

1. Sign-in to jump cloud and proceed to `applications` link
2. Type: `aws` and select `amazon web services` and then `configure` link
3. Open another browser tab/window and proceed to Aws -&gt; Services -&gt; IAM
4. We will need Aws `account number` shown under `AWS account ID:` label
5. JumpCloud Console
   1. For display label type: `Aws Read-Only`
   2. Find the attribute `https://aws.amazon.com/SAML/Attributes/Role`
   3. In the value for this attribute replace `YOUR_AWS_ACCOUNT_NUMBER` with account number copied from Aws console
   4. There is another token we will need to replace later: `ROLE_1`
   5. Select `activate` link and `save`
   6. From the list select the newly configured application
   7. Select ``export metadata` line and save the file``
6. Aws Console
   1. Proceed to Services -&gt; IAM
   2. Select `Identity Providers`
   3. Add new provider of type `SAML`
   4. Type provider name: `JumpCloud`
   5. sdfsdf
   6. And upload the metadata file downloaded from JumpCloud console

   Continue with the next steps until provider has been created

   Proceed 



