# Publish RDS using AAD AP (Azure AD application proxy)

# Requirements

Azure Active Directory

>Azure AD P1/P2 License

>Global administrator role


Active Directory

>Domain controller	* 1 (Windows server 2016+)

>RD Gateway Server * 1 (Windows server 2016+)

>RD session host + broker + licensing - on a single server	* 1 (Windows server 2016+)


The instructions below apply to the scenario where :

>RD gateway	- You don't public IP for RD gateway server

>External url of RD gateway - you don’t have ssl cert issued from public CA, the default external url generated by AAD AP will be used

>Internal url of RD gateway	- You don’t want it to be exposed to public network , external url is different from the internal one



1. Deploy three servers first.
2. Make one server domain controller first.
3. Then join the other two servers to the domain. (do not deploy RDS infrastructure in the beginning )


# P1 : Install AAD AP connector agent on the server which you want to be RD gateway

Navigate to [Microsoft (msappproxy.net)](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) and download the latest version of AAD AP connector

![image](https://user-images.githubusercontent.com/96930989/210068026-24211a12-2ae5-437a-bbdb-216fc7768174.png)


# P2 : Create a new connector group for the new AAD AP connector

1. Navigate to [portal.azure.com](https://portal.azure.com/#home) and sign in with global administrator credentials
2. Navigate to Azure Active Directory > Application proxy
3. Select the server where you installed the connector 
4. Select the area which is close to you to reduce latency 

![image](https://user-images.githubusercontent.com/96930989/210068036-24ae26f7-e9ce-4690-b1c7-c59c80bf986f.png)

5. Click +Create when the settings are done

# P3 : Configure RD Gateway/Web access(via app registration)

1. Navigate to [portal.azure.com](https://portal.azure.com/#home)and sign in with global administrator credentials.
2. Navigate to Azure Active Directory > Enterprise applications , click +new application

![image](https://user-images.githubusercontent.com/96930989/210073463-f534c1f8-77df-4122-b6c0-50442a57d21b.png)

3. Click + Create your own application

![image](https://user-images.githubusercontent.com/96930989/210073481-2284b3cc-3595-40e9-9ea6-b073ad03fd10.png)

Select Configure Application Proxy for secure remote access to an on-premises application

Then click create in the bottom

![image](https://user-images.githubusercontent.com/96930989/210073512-ff316a37-aadc-40da-b6cf-b9db1c6f1dd0.png)


>Internal URL - Suggest to use the internal FQDN of RD gateway server ( normally server name )

>External Url - Leave it if you want to user the default external url created by AAD AP (Note this url and we will need it later)

>Pre Authentication - Select Passthrough so that users will go directly to authenticate with RD web / RD gateway

>Connector group - Select the connector group we newly created

>Backend Application Timeout	- Select Long

>Headers (under Translate URLs in) - Must select no if the external url is different from internal one 

![image](https://user-images.githubusercontent.com/96930989/210073927-a9f06b2e-8664-4f7a-89a4-b39bcdadb736.png)

# P4 : Configuration of RDS infrastructure on the two servers we joined to the domain before
>Detailed steps to deploy RDS role on 2 servers we prepared before https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure

>One server - RD gateway + RD web access

>The other server - RD connector broker + RD session host + RD licensing

>After that , go to Server Manager > Remote Desktop Services > Overview > Tasks > Edit Deployment Properties

![image](https://user-images.githubusercontent.com/96930989/210074129-0d881064-43d7-411c-a7e9-46b9682718d8.png)


1. RD Gateway - Server name - Fill in the external url created by AAD AP in this section

2. Check the box "Use RD Gateway credentials for remote computers"

3. Uncheck the box "Bypass RD Gateway server for local addresses"

4. Double check that RD web access and RD gateway is configured on the same server

![image](https://user-images.githubusercontent.com/96930989/210074383-b3afd92c-0126-4550-bbc9-9d2e8d47e703.png)

# P5 : Configuration of RDS Certificates 

1. Generate certificate 1 by clicking generate new certificate (self-signed certificate using internal FQDN) for : 
>RD gateway 

>RD web access 

Install certificate 1 for RD gateway (we create the cert first then store it locally)
![image](https://user-images.githubusercontent.com/96930989/210074500-76d96dbf-1683-432b-8fc8-323703394b67.png)

Install certificate 1 for RD web access (by clicking select existing certificate and navigate to the cert we just made)
![image](https://user-images.githubusercontent.com/96930989/210074756-3df7f4d8-e5b3-4085-ae68-2ee0b77082b8.png)

Add certificate 1 to RD gateway/web access server's trusted root store  ( The one with client authentication , server authentication )
![image](https://user-images.githubusercontent.com/96930989/210074860-8a12b54a-97d0-4e4e-877b-06f0ac1cba08.png)
![image](https://user-images.githubusercontent.com/96930989/210074893-772f7688-1cf3-4e40-9f10-cfda9e1ac347.png)

2. Certificate 2 ( self-signed certificate using internal FQDN ) for: 
>RD Connection Broker - Enable Single Sign On 

>RD Connection Broker - Publishing services

Cert for RD Connection Broker - Enable Single Sign On
![image](https://user-images.githubusercontent.com/96930989/210075077-6818861c-ee39-47c3-bf89-ca03ccb2aabb.png)

Cert for RD Connection Broker - Publishing services
![image](https://user-images.githubusercontent.com/96930989/210075087-bbde8bda-c498-4150-b031-636c702cb31f.png)

Important !!!  Export the certificate 2 to RD web/gateway server in cer.file
![image](https://user-images.githubusercontent.com/96930989/210075185-052502f0-3f5a-4b8b-9880-637a7338c11c.png)
![image](https://user-images.githubusercontent.com/96930989/210075199-c8578eab-abee-4627-bad8-efafe49b2699.png)
![image](https://user-images.githubusercontent.com/96930989/210075200-ab449332-27ef-4a3e-b6fd-6c44ecc3d49f.png)

# P6 : Configure SSO between RD web and RD gateway

On RD broker server , run powershell command :

Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl/rdweb>`nrequire pre-authentication:i:1"
  
Sample : 

Set-RDSessionCollectionConfiguration -CollectionName "GJSTEST1" -CustomRdpProperty "pre-authentication server address:s:https://chickrdgw-ultramanorb.msappproxy.net/rdweb`nrequire pre-authentication:i:1"
  
![image](https://user-images.githubusercontent.com/96930989/210075295-0eeecd14-06e3-4efe-af4f-27173cbed548.png)

  
Verify the configuration :
  
(get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
  
![image](https://user-images.githubusercontent.com/96930989/210075349-c5145ba8-548d-4386-9dc8-c8fd17ceb93c.png)

  
# P7-Optional : Install and publish RD web client(on RD gateway server)

  Set up the Remote Desktop web client for your users
  
 1. Navigate to RD gateway/web acess server
 2. Launch powershell as admin
 3. Run command : Install-Module -Name PowerShellGet -Force
 4. Restart powershell with admin
 5. Run command : Install-Module -Name RDWebClientManagement
 6. Run command : Install-RDWebClientPackage
 7. Check if the certificate of RD broker server has been copied to RD gateway server (cert 2)
 8. Run ps command : Import-RDWebClientBrokerCert <.cer file path>    

    >Run this cmdlet with the bracketed value replaced with the path of the .cer file that you copied from the RD Broker
  
    Sample
  
    ![image](https://user-images.githubusercontent.com/96930989/210121816-704aaf89-0ec9-44c7-b4d4-219e58ffc8c5.png)

  
    ![image](https://user-images.githubusercontent.com/96930989/210121832-43246df1-e3b7-490d-bf6b-b1e4bb388ac0.png)

 9. Check RDWebClientBrokerCert is configured correctly
  
    >Run PS command below to verify the output shows the information of cert 2:
  
    Get-RDWebClientBrokerCert
  
    ![image](https://user-images.githubusercontent.com/96930989/210121850-857cce0d-bfd2-4269-94eb-c955b520bba4.png)

 10. Publish RD web client using the command below:
  
     Publish-RDWebClientPackage -Type Production -Latest

For more details about this part, please refer to the doc below:

>https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client-admin
  
>https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client-admin#how-to-publish-the-remote-desktop-web-client
  
>https://learn.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-integrate-with-remote-desktop-services#direct-rds-traffic-to-application-proxy





    








  
  
  
  
  
 
  
  
  





