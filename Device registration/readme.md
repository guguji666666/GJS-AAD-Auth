### User certificate verification

### Step 1 : Launch Active Directory Users and computers on domain controller

On domain controller,  Launch Active Directory Users and computers via server manager or dsa.msc

Server Manager

![image](https://user-images.githubusercontent.com/96930989/231020088-36ce2d7d-1c17-46dc-8aa9-6f206368ee0e.png)

Dsa.msc

![image](https://user-images.githubusercontent.com/96930989/231020104-fce56d54-cc90-42fb-ab63-128761dc8da7.png)

Enable Advanced Features

![image](https://user-images.githubusercontent.com/96930989/231020109-2746548f-3a25-47d1-9c64-17d0f90c2e9d.png)


### Step 2 : Navigate to the machine object in Active Directory

Navigate to the Organization Unit that the machine belongs to.

Double click the machine object and click the tab Attribute Editor. find the attribute userCertificate 

![image](https://user-images.githubusercontent.com/96930989/231020131-accdade2-aece-4a23-8470-cf22fd93714c.png)

Double click this attribute and click Edit , you will see the usercertificate in `Hexadecimal` format

![image](https://user-images.githubusercontent.com/96930989/231020161-5f987a2c-35fe-41c0-b36c-e1419aecfcf7.png)

Copy the value and paste to notepad

![image](https://user-images.githubusercontent.com/96930989/231020180-d93a2b34-42fe-4065-9162-85f1d703c108.png)

### Step 3 : Encode user certificate to base64

Navigate to website [Binary to base64: Convert between bytes and base64 — Cryptii](https://cryptii.com/pipes/binary-to-base64)

**Important** : Modify the settings as the picture below :

![image](https://user-images.githubusercontent.com/96930989/231020203-2be8c3c2-000f-47d2-8cec-5425ea0958ae.png)

Paste the certificate on the left, then the certificate will be encoded to base64 and shows on the right

![image](https://user-images.githubusercontent.com/96930989/231020305-37a343d1-e3a8-4b55-8e7e-d1cb1bbd3dc1.png)

### Step 4 : Export the certificate ( encoded to base 64 ) in cer file

Copy the certificate generated to a new notepad

![image](https://user-images.githubusercontent.com/96930989/231020318-712c665f-659d-4363-bd00-a98f2911ef34.png)

Save the file with extension cer

![image](https://user-images.githubusercontent.com/96930989/231020335-6c6187f3-1a58-4860-8df6-50b848159bff.png)

![image](https://user-images.githubusercontent.com/96930989/231020339-1511d95b-8889-4d68-8b7f-b389cb86ab16.png)

Open the cer file created , you will find the cert like the picture below. The values of Issued to and Issued by is device ID of local machine.

![image](https://user-images.githubusercontent.com/96930989/231020365-bae29a88-435a-4232-91d3-5d124f3b49e8.png)

### Step 5 ( Optional ) : Verify the base 64 usercertificate

Navigate to [Certificate Decoder
](https://www.sslshopper.com/certificate-decoder.html)

Copy the base 64 cert to this part ( below is the sample )

![image](https://user-images.githubusercontent.com/96930989/231020412-c0735e33-49f2-4d15-b09e-0ec05b89a78f.png)
