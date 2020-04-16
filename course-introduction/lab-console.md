# Lab: Cloud Console

## Amazon Web Services \(AWS\)

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. Create a new account or login with existing credentials
3. Proceed to `Services` -&gt; `IAM`, location of the link changes over time
4. Follow the links below to educate yourself on the IAM principals
   1. [Root Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) - the owner of AWS account
   2. [IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) - identities to operate AWS account with long-lived credentials
   3. [IAM Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) - a collection of IAM Users
   4. [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) - identities with permissions, but no long-lived credentials
   5. [IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html?icmpid=docs_iam_console) - defines permissions for an identity, e.g.: user group, or resource, e.g. S3 bucket
5. Create a new user and assign it to a user group
6. Open a new incognito browser window to log-in with the newly created user

## Microsoft Azure Cloud \(Azure\)

1. Open a web browser to [https://portal.azure.com/](https://portal.azure.com/)
2. Create a new account or login with existing credentials
3. Search for `Active Directory` and select `Azure Active Directory`
4. Follow the link below to educate yourself on Azure AD principals
   1. [Users](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-users-azure-active-directory?context=azure/active-directory/users-groups-roles/context/ugr-context)
   2. [Groups](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-manage-groups?context=azure/active-directory/users-groups-roles/context/ugr-context)
   3. [Roles based access control](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview)
5. Create a new AD user
6. Open a new incognito browser window to log-in with the newly created user

## Google Cloud Platform \(GCP\)

1. Open a browser to [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. Create a new account or login with existing credentials
3. Search for `IAM` and select `IAM & admin`
4. Follow the link below to educate yourself on GCP IAM principals
   1. There is no option to add a user
   2. [GCP access](https://cloud.google.com/iam/docs/overview) can be granted to any Google account, Google Group, G-Suite user, or Cloud Identity
   3. There is no GCP user groups either, there are Google Groups, G-Suite Groups, and [Cloud Identity Groups](https://cloud.google.com/identity/docs/concepts/groups)
   4. [Roles](https://cloud.google.com/iam/docs/understanding-roles)
5. Grant a new user/group access
6. Open a new incognito browser window to log-in with the newly granted user

### How practical it is to manage a multi-cloud identity and access?

