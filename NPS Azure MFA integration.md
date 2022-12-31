# The configuration appplies to:
1. The RDS has been published via AAD AP or public IP if you had for RD gateway
2. You want the users to involve Azure MFA when reaching the local services published by RDS
3. You need to deploy two more servers for:

>Azure AD connect to sychronize the AD users to Azure AD

>NPS server joined to the same domain

