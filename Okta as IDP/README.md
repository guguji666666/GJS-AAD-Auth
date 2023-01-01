# Okta doc
https://help.okta.com/en-us/Content/Topics/Apps/Office365-Deployment/deploy-main.htm

# Step 1 : Add O365 to applications in Okta portal 
![image](https://user-images.githubusercontent.com/96930989/210168916-9df3d5e1-e3c9-48e2-9015-2ca1b61b7d23.png)
![image](https://user-images.githubusercontent.com/96930989/210168959-fa55dcaa-48f0-4d13-9920-d7cf9bc4ba9c.png)

# Step 2 : Set verified domain in AAD tenant to be federated with Okta
![image](https://user-images.githubusercontent.com/96930989/210169060-95a91881-f77c-4134-ac10-96513702358b.png)

**Sample**
![image](https://user-images.githubusercontent.com/96930989/210169086-808c8a17-e023-4689-b8df-31e2270eeca2.png)
![image](https://user-images.githubusercontent.com/96930989/210169105-03f1cb5a-a86d-48ad-824f-128e95e2417d.png)

**After that, the domain we added becomes federated domain**
![image](https://user-images.githubusercontent.com/96930989/210169122-750b6e16-5910-49bc-8d50-8d7edbb65c89.png)

# Step 3 : Provision users to be synchronized from Okta to Azure AD
**Manually create several users for test**
![image](https://user-images.githubusercontent.com/96930989/210169136-6df6de07-adaa-4bce-9846-e5036f2f7002.png)
![image](https://user-images.githubusercontent.com/96930989/210169148-ed8ddd6d-3e8e-407e-b07b-85c50940fce2.png)

**Provision users from Okta to AAD**
![image](https://user-images.githubusercontent.com/96930989/210169187-0110ad10-8b4f-4da3-beb9-75735406f7ea.png)
![image](https://user-images.githubusercontent.com/96930989/210169193-a5607af2-5e41-4c51-9b3e-90e792211455.png)
![image](https://user-images.githubusercontent.com/96930989/210169199-2d0a72f7-4660-4e8a-a939-cbfaeb82f181.png)
**Click " Authentication with Microsoft Office 365" to get the access token**
![image](https://user-images.githubusercontent.com/96930989/210169211-5e9a7858-03b3-41e2-8901-c87fd8ade386.png)
**Offer the credentials for authorization**
![image](https://user-images.githubusercontent.com/96930989/210169212-571fe63b-614d-42f6-91aa-841cb7e1bdb5.png)
![image](https://user-images.githubusercontent.com/96930989/210169224-d2b2fa26-6d64-40f1-8bbf-8ce9f15df05d.png)

After that , click `Save`
