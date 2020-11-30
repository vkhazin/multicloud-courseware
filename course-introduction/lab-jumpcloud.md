# Lab: Multi-Cloud SSO

## Sample SSO Provider

1. Open browser to [https://jumpcloud.com/](https://jumpcloud.com/)
2. Select `Get Started`
3. Register a new account and an email confirmation may be required

## [AWS Integration](https://support.jumpcloud.com/support/s/article/single-sign-on-sso-with-amazon-aws-in-iam-2019-08-21-10-36-47)

1. Sign-in to jump cloud, proceed to `User Authentication` -> `SSO` link and select plus icon to add a new app
2. Type: `aws` and select `amazon web services` and then `configure` link
3. Open another browser tab/window and proceed to AWS Console -&gt; `Services` -&gt; `IAM`
4. We will need AWS`account number` shown under `AWS account ID:` label
5. JumpCloud Console
   1. For display label type: `AWSRead-Only`
   2. Find the attribute `https://aws.amazon.com/SAML/Attributes/Role`
   3. In the value for this attribute replace `YOUR_AWS_ACCOUNT_NUMBER` with account number copied from AWSconsole, _**TWICE!**_
   4. There is another token we will need to replace later: `ROLE_1`
   5. Select `activate` link and `save`
   6. From the list select the newly configured application
   7. Select `export metadata` line and save the file
6. AWSConsole
   1. Proceed to `Services` -&gt; `IAM`
   2. Select `Identity Providers`
   3. Add new provider of type `SAML`
   4. Type provider name: `JumpCloud`
   5. Upload the metadata file downloaded from JumpCloud console
   6. Continue with the next steps until end of the provider setup
   7. Proceed to `Services->IAM->Roles`
   8. We need to create a new Role to associated with the JumpCloud SSO
   9. Select `Create role` -&gt; `SAML 2.0 federation`
   10. Select the newly created provider and allow console and programmatic access and proceed to `Next: Permissions`
   11. Locate and select `AmazonEC2ReadOnlyAccess` policy
   12. Proceed to `Next:Tags` and then `Next: Review`
   13. Specify Role name as `SsoEc2ReadOnlyAccess` and select `Create role`
7. JumpCloud Console 
   1. `User Authentication` -> `SSO` -> `AWS Read-Only`
   2. Under Single Sing On Configuration section locate attribute `https://aws.amazon.com/SAML/Attributes/Role`
   3. Modify the attribute value to replace `ROLE_1` with the name of the new role we have created in AWS: `SsoEc2ReadOnlyAccess`
   4. Save the changes
   5. Select `Groups`
   6. Create `Group of Users`
   7. For name type: `AwsEC2ReadOnlyAccess`
   8. On `User Authentication` -> `SSO` tab select `AWS Read-Only` application and proceed to `User Groups` link on top
   9. Select the newly created user group
   10. Select `Users` -> `Manual user entry`
   11. Create a new user with the option to `Specify initial password...` selected and provide a password
   12. Select `User Groups` and select the group name created in the previous steps
   13. Select `save user`
8. Incognito Browser Tab/Window
   1. Proceed to [https://console.jumpcloud.com](https://console.jumpcloud.com)
   2. Sign-in with credentials for the new user
   3. Proceed to AWS Console
   4. You should see AWS Console as if you have logged in with IAM credentials
9. Troubleshooting
   1. SAML attributes are finicky: name of the provider, name of the role, and Url must match between the two parties exactly to the case-spelling
   2. Watch out for extra spaces in values

