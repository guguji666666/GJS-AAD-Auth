## Device status in Azure AD

For [`AAD registered`](https://learn.microsoft.com/en-us/azure/active-directory/devices/concept-azure-ad-register)devices running windows OS , you can login the device with the local account on that machine.

For [`Azure AD joined`](https://learn.microsoft.com/en-us/azure/active-directory/devices/concept-azure-ad-join) devices running windows OS , you must login the device with work account ( normally created in Azure AD )

For [`Hybrid Azure AD joined`](https://learn.microsoft.com/en-us/azure/active-directory/devices/concept-azure-ad-join-hybrid) devices running windows OS , you must login the device with domain accounts ( these accounts should also be synchronized to Azure AD )
