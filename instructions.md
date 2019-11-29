#WorkshopPLUS - System Center Configuration Manager: Advanced Concepts and Cloud Services (1906)

##Objective
###Learning Objective - Module 1
The main objective of this session is for the attendees to learn more about Cloud Management Gateway.
###Learning Objective - Module 2
One of the goals of the session will be for the attendees to gain experience and knowledge on how to install, configure and manage co-management within configuration manager. This will include the pre-requirements of Azure, certificates, and cloud management gateway.
###Learning Objective - Module 3
Finally, the attendees will get some hands-on experience with Windows Autopilot, and how to setup and manage a device inside the portal, the attendees will be able to configure a profile and deploy the profiles to the sample devices within Windows Autopilot.

##Scenario
You are working with Contoso customer to implement, Cloud Management Gateway, Co-Management in Configuaration Manager and Autopilot. Let's guide you to the different requirements for a successfull implementation

===
#Module 1, Lab 1, Exercise 1 - Configure EM+S and Azure Tenant

#Scenario
Scenario
In this exercise you will:
- verify your EM+S subscription
- Set up your Azure Account

1. [] Log on to @lab.VirtualMachine(NYCCL1).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Go to +++https://portal.office.com+++ 
1. [] Click *Resources* tab, in the lab menu, and use the LAB Office 365 credentials specified under your Lab environment
!IMAGE[rr8li18k.jpg](rr8li18k.jpg)

	> [!NOTE] These are your tenant credentials and you shall use it every time you sign-in a Cloud portal as admin

1. [] At *Stay signed in*, select *Don't show this again* and click **Yes**
!IMAGE[1rq5rfi9.png](1rq5rfi9.png)
1. [] Navigate to Billing and click *Licenses*
!IMAGE[1Capture.PNG](1Capture.PNG)
1. [] and confirm that you have available **Enterprise Mobility + Security E5** licenses
!IMAGE[2Capture.PNG](2Capture.PNG)
1. [] Go to Users > Active users
!IMAGE[3Capture.PNG](3Capture.PNG)
1. [] Click *MOD Administrator*
!IMAGE[4Capture.PNG](4Capture.PNG)
1. [] Click *Licenses and Apps*, select *Enterprise Mobility + Security E5* and then click *Save changes*
!IMAGE[5Capture.PNG](5Capture.PNG)
1. [] Close the *Licenses and Apps* blade and confirm that the EM+S license is now assigned to the MOD Administrator
!IMAGE[7Capture.PNG](7Capture.PNG)
1. [] Open a new tab in the browser and go to +++http://www.microsoftazurepass.com/+++
1. [] Sign in (with your LAB Office365 credentials) - the **SIGN IN** link is in the top right corner of the page
!IMAGE[ulhsc8lm.jpg](ulhsc8lm.jpg)
1. [] Click **Confirm Microsoft Account**
!IMAGE[Screenshot](Screens/wu35xgsn.jpg)
1. [] Then paste the Azure Pass promo code specified under your Lab environment - *Resources* tab - and then click **Claim Promo Code**.
!IMAGE[Screenshot](Screens/jgcmfsel.jpg)
1. [] Wait a few moments, then click **Next** without changing any information
!IMAGE[Screenshot](Screens/gw2yioor.jpg)
1. [] Accept the Agreement and click **Sign-up**
!IMAGE[Screenshot](Screens/ts2ery3x.jpg)
>[!NOTE] if you see this message just be patient, this is very common, it will eventually work (approx. 5 minutes).
!IMAGE[uc0c78oh.jpg](uc0c78oh.jpg)
17. [] Select **Continue to Azure Portal website**
!IMAGE[zuu3zj6m.jpg](zuu3zj6m.jpg)
1. [] The Azure portal (https://portal.azure.com) will automatically open, click **Maybe later** to continue
!IMAGE[Screenshot](Screens/ktk1ewes.jpg) 
1. [] Navigate to All Services > Subscriptions
!IMAGE[66mw3reo.png](66mw3reo.png)
1. [] Click Subscriptions and confirm you have a valid Azure Pass
!IMAGE[4.PNG](4.PNG)

Congratulations! 
You have successfully:
- Set up your tenant in Azure


Click Next > to advance to the next exercise.
====
#Module 1, Lab 1, Exercise 2 - Create Certificates for Cloud Management Gateway

##Scenario
In this exercise you will:
- Create a Certificate for the CMG from a Local Certificate Authority

> [!ALERT] Since we are working with certificates is **paramount** that the local VMs' time is correct. Confirm the local time is correct and change the time zone as needed.

1. [] Stay on @lab.VirtualMachine(NYCCL1).SelectLink and right-click **Start**, click **Run**, and then enter +++certlm.msc+++. 
1. [] In the console, expand Certificates (Local Computer), and then choose **Personal**
!IMAGE[Screenshot](Screens/z5hgiv0q.jpg)
1. [] Right-click Certificates, choose All Tasks, and then choose **Request New Certificate**
!IMAGE[Screenshot](Screens/un3vfyie.jpg)
1. [] At Before You Begin page, choose **Next**
!IMAGE[Screenshot](Screens/53bxjkzn.jpg)
1. [] At Select Certificate Enrollment Policy page, choose **Next**
!IMAGE[Screenshot](Screens/wusw5525.jpg)
1. [] On the Request Certificates page, identify the *ConfigMgr Cloud Server Certificate* from the list of available certificates, and then choose **More information is required to enroll for this certificate. Click here to configure settings**
!IMAGE[Screenshot](Screens/f5b0cylw.jpg)
1. [] In the Certificate Properties dialog box, in the Subject tab, for the Subject name, choose **Common name** as the Type. 
!IMAGE[Screenshot](Screens/hcrc5hbs.jpg)
1. [] In the Value box, specify your choice of service name and your domain name by using an FQDN format. 
For example, use: <3digitairportcode+todaysdate+yourinitials>.contoso.com.contoso.com e.g. MAD21052018ac.contoso.com
1. [] Choose **Add**, and then choose **OK** to close the Certificate Properties dialog box
!IMAGE[2c9olya1.jpg](2c9olya1.jpg)
1. [] On the Request Certificates page, choose ConfigMgr Cloud Server Certificate from the list of available certificates, and then choose **Enroll**
!IMAGE[Screenshot](Screens/2oauj5c1.jpg)
1. [] On the Certificates Installation Results page, wait until the certificate is installed, and then choose **Finish**.

> [!ALERT] You will use DNS to create an alias (CNAME record) to map this service name (e.g. MAD21052018ac.contoso.com) to a Cloud Service in Microsoft Azure.

12. [] In the Certificates (Local Computer) console, right-click the certificate that you have just enrolled, choose All Tasks, and then choose Export
!IMAGE[Screenshot](Screens/vwhgubpa.jpg)
1. [] In the Certificates Export Wizard, choose **Next**
!IMAGE[xn1jm2l8.png](xn1jm2l8.png)
1. [] On the Export Private Key page, choose **Yes, export the private key**, export the private key, and then choose **Next**
!IMAGE[Screenshot](Screens/11ahs4zq.jpg)
1. [] On the *Export File Format* page, ensure that the Personal Information Exchange - PKCS #12 (.PFX) option is selected, leave the defaults and click **Next**
!IMAGE[Screenshot](Screens/f3vokpuh.jpg)
1. [] On the Password page, specify +++Pa$$w0rd+++ to protect the exported certificate with its private key, and then choose **Next** (leave default for encryption)
!IMAGE[1fkdeav9.png](1fkdeav9.png)
1. [] On the File to Export page, specify the name of the file that you want to export, and then choose **Next**. The file name to use is: +++contosocmg-pk.pfx+++
!IMAGE[Screenshot](Screens/spcsxyld.jpg)
1. [] To close the wizard, choose **Finish** in the Certificate Export Wizard page, and then choose **OK** in the confirmation dialog box. 
!IMAGE[Screenshot](Screens/vke0p0kb.jpg)

> [!ALERT] Store the file securely and ensure that you can access it from the System Center Configuration Manager console. The certificate is now ready to be imported when you create a Cloud Management Gateway.

> [!ALERT] The Cloud Management Gateway short name must be unique in Azure. To ensure you are using a unique name go to +++https://portal.azure.com+++ then navigate to **+Create a Resource**, search for+++Cloud Service+++ and click **Create**
!IMAGE[5.PNG](5.PNG)
and verify that the DNS name is available in your Location (e.g. West Europe).
This is an example of when it is not available
!IMAGE[lo46jr62.png](lo46jr62.png)
This is an example of when it is available
!IMAGE[agvk0syw.png](agvk0syw.png)
**IMPORTANT: Do not create the cloud service, this is step is only for check DNS name availability. Simply click the X to close the blade**
!IMAGE[auh1kpk8.jpg](auh1kpk8.jpg)
**Then click OK**
!IMAGE[oqx6ih3z.jpg](oqx6ih3z.jpg)

Congratulations! 
You have successfully:
- Completed the Certificate Creation.

Click Next > to advance to the next exercise.
===
#Module 1, Lab 1, Exercise 3 - Export the Root CA

##Scenario
In this exercise you will:
- Export Root CA to C Drive

> [!KNOWLEDGE] We are only exporting the Root CA for this lab since we don't have an Intermediate Root CA, is important that if is available all Root CA or Intermediate Root CA be exported and imported later on to the CMG.

1. [] Stay on the *Certificates* console (+++certlm.msc+++)
1. [] Expand *Trusted Root Certification Authority* > Certificates
!IMAGE[Screenshot](Screens/xlhzc3ip.jpg)
1. [] Right Click on contoso-NYCDC-CA as shown in the screenshot, then select **All Task** > **Export**
!IMAGE[Screenshot](Screens/yyba3odl.jpg)
1. [] On the Certificate Export Wizard click **Next**
1. [] On the *Export File Format* leave default (DER encoded binary) and click **Next**
1. [] On the File to Export Name type +++ContosoRootCA.CER+++ and click **Next**
1. [] Click **Finish** and then **Ok**
1. [] Close Certificates (Local Computer).

Congratulations! 
You have successfully:
- Exported the Root CA.
Click Next > to advance to the next exercise.

===
#Module 1, Lab 2, Exercise 1 - Adding an Additional UPN Suffix

##Objectives
After completing this exercise, you will be able to:
- Add an additional UPN suffix to your local domain that matches your tenant.

1. [] Log on to @lab.VirtualMachine(NYCDC).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Click Start, open Server Manager
!IMAGE[lqvqgadg.png](lqvqgadg.png)
then under tools select **Active Directory Domains and Trusts**
!IMAGE[Screenshot](Screens/5q5x40nb.jpg)
1. [] In the Active Directory Domains and Trusts snap-in, click Action on the toolbar and then click Properties
!IMAGE[Screenshot](Screens/hcbg4zlj.jpg)
1. [] This opens the UPN Suffixes Tab. Enter the domain name that you find under Resources tab > Office 365 credentials (TenantName) (e.g. LODxxxxxxxx.onmicrosoft.com)
and click **Add**. 
!IMAGE[y59895ag.png](y59895ag.png)
1. [] Click OK in the UPN Suffixes tab and close Active Directory Domains and Trusts
1. [] From Server Manager > Tools open **Active Directory Users and Computers**
!IMAGE[Screenshot](Screens/wypuld2x.jpg)
1. [] In Active Directory Users and Computers, expand contoso.com and click on the *Workshop Accounts* OU
1. [] Select the user **Mark Steele**
1. [] Right-click Mark Steele and click **Properties**
1. [] Click the Account tab and change the UPN suffix of the user to the Microsoft Office 365 domain name <Domain Name>.onmicrosoft.com that you added in Active Directory Domains and Trusts
!IMAGE[Screenshot](Screens/ynmjpysd.jpg)
1. [] Click OK.

Congratulations! 
You have successfully:
- Add an additional UPN suffix to your local domain that matches your tenant.
Click Next > to advance to the next exercise.
===
#Module 1, Lab 2, Exercise 2 - Enabling Azure Active Directory Synchronization (installing and configuring Azure AD Connect for first use)

##Scenario
In this exercise you will: 
- Configure AD Connect.

1. [] Log on to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Open the web browser and go to +++https://www.microsoft.com/en-us/download/details.aspx?id=47594+++ and click **Download** to get the latest version of Azure AD Connect.
!IMAGE[Screenshot](Screens/3yx2q2v3.jpg)
1. [] Click **Run** as the action when the download completes.
!IMAGE[Screenshot](Screens/13k3g0u3.jpg)
1. [] On the Welcome to Azure AD Connect page, select the terms and privacy notice (I agree to the license terms and privacy notice) and click **Continue**
!IMAGE[Screenshot](Screens/2hen4qbe.jpg)
1. [] Select Customize
!IMAGE[Screenshot](Screens/zhxzr3j0.jpg)
1. [] On the required Components Select 
	- Use and existing SQL server and enter +++NYCCFG+++ as the name
	- Use an existing service account. Enter +++Contoso\administrator+++ and +++Pa$$w0rd+++
	!IMAGE[Screenshot](Screens/zra30qfz.jpg) 
	 then click **INSTALL**

> [!ALERT] In a production environment you will never use a domain admin account for this purpose, we are using Contoso\administrator just for simplicity in this Lab.

7. [] On the User Sign-in select *Password Hash Synchronization* and click **Next**
!IMAGE[Screenshot](Screens/pjxodvlj.jpg)
1. [] On the Connect to Azure AD, enter  the LAB Office365 credentials, then click **Next**
!IMAGE[Screenshot](Screens/mdnaa0eo.jpg)
1. []  On the connect your directories click **Add Directory** then select Use Existing AD Account, 
!IMAGE[c7bfzhj3.jpg](c7bfzhj3.jpg)
enter +++Contoso\administrator+++ and +++Pa$$w0rd+++ and click **OK**, then click **Next**
1. []  On the Azure AD sign-in configuration select **Continue without matching all UPN Suffixes to verified domains** and click **Next**
!IMAGE[aqtkx4sp.jpg](aqtkx4sp.jpg)
1. [] On the Domain and OU filtering click **Next**
1. [] On the Uniquely identifying your users click **Next**
1. [] On Filter users and devices click **Next**
1. [] On the Optional features click **Next**
!IMAGE[Screenshot](Screens/vj34js5i.jpg)
1. [] At Ready to configure click **Install**
!IMAGE[p3b22x5h.jpg](p3b22x5h.jpg)
1. [] Wait for the installation to complete then click **Exit** (it will take a couple of minutes)
!IMAGE[Screenshot](Screens/lpszuztv.jpg)

1. [] Log on to @lab.VirtualMachine(NYCCL1).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Wait approx. 5 minutes then open +++https://portal.office.com+++ and navigate to Admin > Users > Active Users and confirm Mark Steele has been synchronized
!IMAGE[skvwa9d8.png](skvwa9d8.png)
1. [] Select Mark Steele, click "License and Apps" tab. Select location as "United States", select and enable "Enterprise Mobility + Security E5".
!IMAGE[zmlul7np.jpg](zmlul7np.jpg)
1. [] Click "X" at top right corner to close the window.

Congratulations! 
You have successfully:
- Configured AD Connect.
- Assigned an EM+S E5 license to a user in Active Directory synchronized with Azure AD.

Click Next > to advance to the next exercise

===
#Module 1, Lab 3, Exercise 1 -  Configure Azure AD User and Group Discovery

##Scenario
In this exercise you will:
- On Board Azure Tenant. 
- Create Server Application 
- Create Client Application 
- Approve Permissions 
- Review Azure Discovery Agent Log Files

1. [] Log on to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Open the Configuration Manager Console 

>[!ALERT] Go to Monitoring > Database Replication - if you see that the *link has failed* restart the NYCCFG first and then NYCCFG
!IMAGE[kqs3gylx.jpg](kqs3gylx.jpg) 
**DO NOT CONTINUE UNTIL THE PROBLEM IS RESOLVED**
!IMAGE[hg4jtlop.jpg](hg4jtlop.jpg)
Hit refresh and check that the problem is resolved
NYCCFG
!IMAGE[mte602ud.jpg](mte602ud.jpg)
NYCCFG
!IMAGE[1i7n0835.jpg](1i7n0835.jpg)

3. [] Go to Administration Workspace.
1. [] Expand Cloud Services 
1. [] Right-click on *Azure Services* and select *Configure Azure Services*
!IMAGE[Screenshot](Screens/13pj10l1.jpg)
1. [] Type +++Contoso Corporation+++ for the *Name*
1. [] Type +++This is a Test+++ for the *Description*
1. [] Leave Cloud Management Selected and click **Next**
1. [] On App Properties (Web app) click **Browse...** next to *Web app* to create the Server Application
!IMAGE[Screenshot](Screens/lskjyxoe.jpg)
1. [] On the Server Application click **Create...**
!IMAGE[Screenshot](Screens/wegcnnh2.jpg)
	- Application Name: +++Contoso Azure+++
1. [] Then **Sign in...** as tenant admin
1. [] Once the *Signed-in is successfully!* click **OK**
!IMAGE[zd2ou47m.png](zd2ou47m.png)

>[!ALERT] If you get a failure *Sign in to Azure* event when you triple-checked that the credentials are correct
!IMAGE[Capture6.PNG](Capture6.PNG)
proceed as follow:
1. [] Open Internet Explorer and go to +++https://portal.azure.com+++
1. [] Log-in as your tenant admin
1. [] Click on Azure Active Directory > App registrations
!IMAGE[l9e1akfc.jpg](l9e1akfc.jpg)
1. [] Click on **Contoso Azure**
!IMAGE[njjed5tv.jpg](njjed5tv.jpg)
1. [] Open Notepad
1. [] Copy the ApplicationID and ObjectID into Notepad
!IMAGE[odtnjisg.jpg](odtnjisg.jpg)
1. [] Click on Certificates & secrets, click "+ New client secret"
1. [] Enter +++ConfigMgr+++ as key description and change the duration to 1 year then click Save
!IMAGE[8ft2gey1.jpg](8ft2gey1.jpg)
1. [] Copy the generated key into Notepad
!IMAGE[5qwmpp4w.jpg](5qwmpp4w.jpg)
1. [] Go to API Permission 
!IMAGE[y1bjh8ne.jpg](y1bjh8ne.jpg)
1. [] Click on +Add a permission
1. [] Select **Microsoft Graph**
!IMAGE[za0pglpk.jpg](za0pglpk.jpg)
1. [] At *Request API Permissions* select *Directory.Read.All* (It is previously called "Read direcotory data") under **Application Permissions** type
!IMAGE[06mh0pak.jpg](06mh0pak.jpg)
1. [] Click 'X' to close the window
1. [] Go to Azure Active Directory > Properties and copy the Directory ID into Notepad as well
!IMAGE[q3wyayr8.jpg](q3wyayr8.jpg)
1. [] Go back to the configuration Console and click **OK** then the *Failed to Sign in to Azure* window
1. [] Click *Cancel* to go back to the Server App windows and then click Import...
!IMAGE[a37b4mfe.jpg](a37b4mfe.jpg)
1. [] At *Azure AD Tenant Name* type +++Contoso+++
1. [] At *Azure AD Tenant ID* paste the Directory ID
1. [] At *Application Name* type +++Contoso Azure+++
1. [] At *Client ID* paste the Application ID
1. [] At *Secret key* paste the Key
1. [] At *Secret Key Expiry* change it to 1 year from now
1. [] Click Verify then click **Ok**
!IMAGE[1bp65dos.jpg](1bp65dos.jpg)

13. [] Select the App in the list and click OK
!IMAGE[foaz7kvf.png](foaz7kvf.png)
1. [] On the App Properties (Native Client app) click **Browse...** on the Native Client App
1. [] On the Client App click **Create...**
1. [] On the Create Application specify +++Contoso Native Client+++ in the *Application Name*
1. [] Then **Sign in...** as tenant admin
1. [] Once the sign-in is successful click OK
!IMAGE[ya3qwkbg.png](ya3qwkbg.png)

> [!ALERT] If you get an error creating the client app
!IMAGE[pv6qecfu.jpg](pv6qecfu.jpg)
proceed as follow:
1. [] Open Internet Explorer and go to +++https://portal.azure.com+++
1. [] Log-in as your tenant admin
1. [] Click on Azure Active Directory > App registrations (Legacy)
1. [] Select +New Application Registration
1. [] At Name type +++Contoso Native Client+++
1. [] Change the Application type to Native
1. [] At Redirect URL type +++https://ConfigMgrClient+++
1. [] Click Create
!IMAGE[id25ljgd.jpg](id25ljgd.jpg)
1. [] Copy the Application ID to Notepad
!IMAGE[hr8f51pl.jpg](hr8f51pl.jpg)
1. [] Click on Settings > Required permission > +Add
!IMAGE[vg45ex6u.jpg](vg45ex6u.jpg)
1. [] Select *Windows Azure Active Directory*
!IMAGE[x1kiu768.jpg](x1kiu768.jpg)
1. [] At *Select permission* choose *Sign in a read user profile*
!IMAGE[e7o65gkv.jpg](e7o65gkv.jpg)
1. [] Click **Done**
1. [] At *Required permission* Click **+Add**
1. [] Search for *Contoso Azure* and select it
!IMAGE[umuhue6t.jpg](umuhue6t.jpg)
1. [] Click on *Select permission* then choose *Access Contoso Azure*
!IMAGE[1mqoipot.jpg](1mqoipot.jpg)
1. [] Click **Done**
1. [] Go to Settings > Redirect URIs and replace *https://ConfigMgrClient** with +++ms-appx-web://Microsoft.AAD.BrokerPlugin/+++
1. [] Append to the URI the Application ID copied from the Notepad, so the end result is like this
!IMAGE[mwclcuky.jpg](mwclcuky.jpg)
1. [] Click **Save**
1. [] Go back to the ConfigMgr Console
1. [] Click **Ok** to Failed to Create Client App windows
1. [] Click Cancel to go back to the Client App windows, then click Import...
!IMAGE[lc84yq2m.jpg](lc84yq2m.jpg)
1. [] At *Application Name* type +++Contoso Native Client+++
1. [] At Client ID paste the Application ID, then click **Ok**
!IMAGE[dhk8d6k5.jpg](dhk8d6k5.jpg)


19. [] Select the newly created application and click **OK**
!IMAGE[6kwl8ob9.png](6kwl8ob9.png)
1. []  After this step both applications are created, click **Next** to continue.
!IMAGE[Screenshot](Screens/qruyihok.jpg)
1. [] Log on to @lab.VirtualMachine(NYCCL1).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Go to +++https://portal.azure.com+++ and log in with your LAB Office365 credentials
1. [] Navigate to Azure Active Directory > App registrations (Legacy) and change the drop down to **All apps**
!IMAGE[pcrc453q.jpg](pcrc453q.jpg)
1. [] Select the server application (**Contoso Azure**) > Settings > Required permissions > Grant permissions 
!IMAGE[Screenshot](Screens/tbdfz1pw.jpg) and click **Yes** to confirm
1. [] Go to Azure Active Directory > App registrations (Legacy) 
1. [] Select the client application (**Contoso Native Client**) > Settings > Required permissions > Grant permissions 
!IMAGE[Screenshot](Screens/dq43xanu.jpg) and click **Yes** to confirm
> [!ALERT] Note: Once you've granted permissions there is a wait period before the permissions are fully applied, wait around 5 minutes before proceeding with the next step.

27. [] Connect to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] On the Configure Discovery Settings, leave as default and click **Next** twice then **Close**
!IMAGE[Screenshot](Screens/lno21bf2.jpg)

1. [] In the Configuration Manager Console,  go to Administration Workspace > Cloud Services > Azure Services 
1. [] On the ribbon click on Run Full Discovery Now > then click yes
!IMAGE[Screenshot](Screens/igxrpd3n.jpg)
1. [] Open Windows Explorer
1. [] Open +++E:\\Program Files\\Microsoft Configuration Manager\\Logs\\SMS_AZUREAD_DISCOVERY_AGENT.log+++ and verify that the syncronization ran correctly.
!IMAGE[wkiusv92.jpg](wkiusv92.jpg)
1. [] You can also check in the console that the sync succeded 
!IMAGE[1nzjqc72.jpg](1nzjqc72.jpg)

Congratulations! 
You have successfully:
- Configure AD Users and Group Discovery

Click Next > to advance to the next exercise.

===
#Module 1, Lab 3, Exercise 2 - Install Cloud Management Gateway

##Scenario
In this exercise you will:
- Install and Configure Cloud Management Gateway - ARM

### Confirm that the required Cloud Resource providers are Registered

1. [] Log on to @lab.VirtualMachine(NYCCL1).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Open +++https://portal.azure.com+++
1. [] Navigate to All Services > Subscriptions (select the curent subscription)> Resource Providers
!IMAGE[registration.PNG](registration.PNG)
1. [] Search for +++Microsoft.Storage+++
!IMAGE[xzj42frh.jpg](xzj42frh.jpg)
and click Register
1. [] Search for +++Microsoft.ClassicStorage+++ and click Register
1. [] Search for +++Microsoft.ClassicCompute+++ and click Register
1. [] wait for the registrations to complete
!IMAGE[registered.PNG](registered.PNG)


###  Cloud Management Gateway Deployment
1. [] Go to Administration Workspace on the Configuration Manager Console.
1. [] Navigate to Updates and Servicing > Features
1. [] Verify Cloud Management Gateway feature is turned ON
!IMAGE[Screenshot](Screens/yq2w5yra.jpg)
1. [] Go to Administration workspace > Cloud Services 
1. [] Right-click on Cloud Management Gateway and click **Create Cloud Management Gateway**
1. [] On Subscription admin account click **Sign-In...** and enter your Office365 Global Admin account (e.g. admin@lod....onmicrosoft.com)
1. [] Wait until the App Information will be populated and click **Next**
1. []  On the Settings page of the wizard, first click Browse and select the .PFX file for the CMG server authentication certificate. The name from this certificate populates the required Service FQDN and Service name fields. Type +++\\\\NYCCL1\C$\\USers\\Administrator\\contosocmg-pk.pfx+++ and click **Open**
1. [] Type +++Pa$$w0rd+++ for the password and click **OK**
!IMAGE[Screenshot](Screens/w2obnrtc.jpg)
1. [] Change the Resource Group to **Create New**
1. [] Click on **Certificates...** and then click **Add** and browse for the +++\\\\NYCCL1\C$\\Users\\Administrator\\ContosoRootCA.cer+++ and click **Open**, then click **OK**
!IMAGE[Screenshot](Screens/pykfvvbd.jpg)
1. [] UNCHECK the **Verify Client Certificate Revocation** 
1. [] Check **Allow CMG to function as cloud distribution point..** and click **Next**
!IMAGE[vcqj2i9g.jpg](vcqj2i9g.jpg)
1. [] Leave default and click **Next** twice then **Close**


Configuration Manager starts setting up the service. After you close the wizard, it will take between five to 15 minutes to provision the service completely in Azure. Check the Status column for the new CMG to determine when the service is ready. 
!IMAGE[m0lg8chn.jpg](m0lg8chn.jpg)

> [!KNOWLEDGE] To troubleshoot CMG deployments, use +++E:\Program files\Microsoft Configuration Manager\Logs\CloudMgr.log+++ file.

Congratulations! 
You have successfully:

- Installed and Configure CMG.

Click Next > to advance to the next exercise.

===
#Module 1, Lab 3, Exercise 3 -  Configure DNS Records

##Scenario
In this exercise you will:
- Configure Internal DNS Records.

1. [] Log on to @lab.VirtualMachine(NYCDC).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Go to Server Manager > Tools > DNS 
!IMAGE[Screenshot](Screens/ue1dcw4a.jpg)
1. [] Expand NYCDC > Forward Lookup Zones > contoso.com 
1. [] Right-click New Alias (CNAME)
!IMAGE[Screenshot](Screens/5bocswug.jpg)
1. [] Alias Name: type the short name of the CMG e.g. mad21052018ac

> [!KNOWLEDGE] if you don't remember this is the value under Configuration Manager console > Administration > Overview > Cloud Services > Cloud Management Gateway (Cloud Service Name)
!IMAGE[kb4i62ua.png](kb4i62ua.png)

1. [] Fully qualified name: type the FQDN for the CMG e.g. mad21052018ac.**cloudapp.net** and click OK
1. [] Open the command prompt and run a nslookup for the short names you provided in the CNAME and verify they can be resolved
!IMAGE[q7ngpq97.jpg](q7ngpq97.jpg)

Congratulations! 
You have successfully:

- Configured DNS Records.

Click Next > to advance to the next exercise.
===
#Module 1, Lab 3, Exercise 4 -   Configure the Management Point for Gateway Connector

##Scenario
In this exercise you will:
- Configure Management Point as Gateway for Cloud Management.

1. [] Log on to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++

1. [] Go Administration Workspace > Site Configuration > Servers and Site Systems Roles, and select NYCCFG
!IMAGE[Screenshot](Screens/fqgz3crq.jpg)
1. [] Under Site system roles, right-click Management point > Properties.
!IMAGE[Screenshot](Screens/m3t4purk.jpg)
1. [] Select Allow Configuration Manage cloud management gateway traffic and then hit **OK**
!IMAGE[Screenshot](Screens/fjp2efil.jpg)
1. [] Under Site system roles, right-click Software Update Point > Properties
1. [] Select Allow Configuration Manage cloud management gateway traffic and then hit **OK**
!IMAGE[8pw34gtx.jpg](8pw34gtx.jpg)
1. []  Go to Administration Workspace > Site Configuration > *Site and System Roles* and select NYCCFG
1. [] Select Add Site System Roles
!IMAGE[Screenshot](Screens/0onbwsrt.jpg)
1. [] At the Add site system roles wizard click **Next** 
1. [] At Proxy click **Next**
1. [] On the Specify roles for this server, select *Cloud management gateway connection point* and click **Next**
!IMAGE[Screenshot](Screens/zigy3zzy.jpg)
1. [] On the *Specify Cloud management gateway connection point settings*, review the option offered then click **Next** twice and then click **Close**
!IMAGE[Screenshot](Screens/pfdnburb.jpg)
1. [] Wait approx. 3 minutes, then monitor the creation of the log +++\\\\NYCCFG\\sms_nyc\\Logs\\SMS_CLOUD_PROXYCONNECTOR.log+++ It should take approx. 5 minutes to stabilize (ignore the errors like *Failed to build Http connection*
!IMAGE[e4dv5cvx.jpg](e4dv5cvx.jpg)
that you will see in the log for approx. 10 minutes), 
when the connection is working it looks like in this picture (Connected)
!IMAGE[cgsq15k6.jpg](cgsq15k6.jpg)
1. [] Go to Administration > Overview > Cloud Services > Cloud Management Gateway and click Connection analyzer in the ribbon
!IMAGE[241fvqfh.jpg](241fvqfh.jpg)
1. [] Click **Sign-in...** and use your Global Admin account and confirm it was successful
!IMAGE[91ftpwnd.jpg](91ftpwnd.jpg)
1. [] Click **Start** and confirm all the test are OK
!IMAGE[hrt73dwk.jpg](hrt73dwk.jpg)

Congratulations! 
You have successfully:
- Configured the Management Point for Gateway Connector and completed Module 1 Labs.

Click Next > to advance to the next Module.

===
#Module 2, Lab 1, Exercise 1 -  Enable users for Co-Management and Autopilot

##Scenario
In this exercise you will:
- Create a new group in Active Directory.

1. [] Log on to @lab.VirtualMachine(NYCDC).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. []  On Server Manager Click Tools then Active Directory Users and Computers. 
1. [] Right-click on Workshop Accounts > New > Group
!IMAGE[Screenshot](Screens/oc2da5uo.jpg)
1. [] Type +++Lab SCCM Install Group+++ as group name and click OK
1. []  Right-click the new group and select properties > Members > click add and enter **Mark Steele** and click OK
!IMAGE[Screenshot](Screens/r130rfs0.jpg)

You have successfully:

- Created a users' group that will be used for co-management.

Click Next > to advance to the next exercise.
===
#Module 2, Lab 1, Exercise 2 -   Create and Deploy Client Settings for Cloud Services

##Scenario
In this exercise you will:
- Create a Cloud Service Settings for Device
- Create a Cloud Service Settings for Users 
- Create Pilot - Co-Management Collection

1. [] Log on to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Open Configuration Manager Console > Administration Workspace > Client Settings
1. [] Right Click Client Settings and Create a Custom Client Device Settings 
1. [] Type +++LAB Device Setting+++ as Name
!IMAGE[Screenshot](Screens/bl0ytp53.jpg)
1. [] Select Cloud Services, then on the corner click on Cloud Services and set everything to yes, then click **OK** 
!IMAGE[Screenshot](Screens/qwb33qww.jpg)
1. [] Right Click Client Settings and Create a Custom Client User Settings 
1. [] Type +++LAB User Setting+++ as Name
!IMAGE[Screenshot](Screens/cisxjnyo.jpg)
1. [] Select Cloud Services, then on the corner click on Cloud Services, change the *Allow access to cloud distribution point* from No to **Yes** 
!IMAGE[Screenshot](Screens/pxz3dzvr.jpg)
1. [] Then click **OK**
1. [] On the  Configuration Manager Console  go to  Assets and Compliance Workspace > Device Collection 
1. [] Right Click on Device Collection and Click Create Device Collection:
	- Name: +++Pilot Co-Management+++
	- Comment: +++This collection will be used for Pilot and Testing Co-management+++
	- On Limiting Collection click Browse and Select All Desktop and Server Clients Then Click Ok. 

1. [] Click **Next**
1. [] Click **Add Rule** and select **Query rule**
1. [] Type +++RS3++++ as Name
1. [] Click on **Edit Query statement**
1. [] Click on **Criteria** tab and then the starburst icon
!IMAGE[Screenshot](Screens/kr5xelju.jpg)
1. [] Click on **Select**
	- Attribute class: *Operating System*
	- Attribute: *Version*
	!IMAGE[Screenshot](Screens/yi4fxojo.jpg)
1. [] Click **OK**
1. [] Select *is greater than or equal to* and type +++10.0.16299+++ and click **OK**
1. [] Click on the starburst icon
1. [] Click **Select**
	- Attribute class: *Operating System*
	- Attribute: *Caption*
1. [] Click **OK**
1. [] Select *is like* and type +++Microsoft Windows 10%+++ and click **OK**
!IMAGE[Screenshot](Screens/h21vqncw.jpg)
1. [] Click **OK** twice
1. [] Select *Use incremental updates for this colection* and deselect *Schedule a full update on this collection*
!IMAGE[Screenshot](Screens/nne23ayc.jpg)
1. [] Click **Next** twice and then **Close**
1. [] Go to Administration Workspace > Client Settings 
1. [] Right Click on *Lab Device Setting* and Select Deploy on the Select Collection Select **Pilot Co-Management** and Click **OK**
1. [] Right Click on *Lab User Setting* and Select Deploy on the Select Collection Select **All Users** Collection and Click **OK**

Congratulations! 
You have successfully:

- Created Client Settings for Cloud Services

Click Next > to advance to the next exercise.
===
#Module 2, Lab 1, Exercise 3 - Prepare Intune for co-management

1. [] Log on to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] In the Configuration Manager console, go to Administration > Overview > Cloud Services > Co-management
1. [] On the Home tab, in the Manage group, choose Configure co-management to open the Comanagement Onboarding Wizard
!IMAGE[Screenshot](Screens/qyd5j002.jpg)
1. []  On the Subscription page, click **Sign In** and sign in to your Global Admin account, and then click **Next**
!IMAGE[Screenshot](Screens/qu33c55o.jpg)
1. [] On the Enablement page, click Copy in the *Devices enrolled in Intune* section to copy the command line to the clipboard, and then save the command line to use in the procedure to create the app.
!IMAGE[l24q5rn4.png](l24q5rn4.png)
1. [] Open Notepad.exe and paste the command line there, 
1. [] Add the text  +++/NoCRLCheck+++ (+space) after the **="** command like in the picture
!IMAGE[mep2sgij.png](mep2sgij.png)
1. [] After /NoCRlcheck (+space) add the text +++/mp:https://+++ followed but the same value of what is inside *CCMHOSTNAME=* (e.g. server.contoso.com/CCM_Proxy_MutualAuth/7)
!IMAGE[6pec66et.jpg](6pec66et.jpg)
1. [] Add the text +++CCMALWAYSINF=1+++ (+space) before SMSSiteCode
!IMAGE[4sutl42k.jpg](4sutl42k.jpg)

> [!KNOWLEDGE] Adding **/NoCRLCheck** is needed because we are using internal PKI certificates and the CRL is not published on the Internet 
**CCMALWAYSINF=1** is needed to force the client to use the CMG even when they are in the LAN - this is needed for lab purposes only

9. [] Save it as +++C:\\Windows\\temp\\Intunecmd.txt+++ so you can use this later on the lab. 

1. [] Go back to the ConfigMgr console, click **Next** and at *Workloads* change *Endpoint Protection* to *Pilot Intune* and click **Next**
!IMAGE[acd33avg.png](acd33avg.png)
1. [] At *Configure Roll out Collections*, browse and select **Pilot Co-Management** device collection
!IMAGE[Screenshot](Screens/rkjqrgac.jpg)
1. [] Click **Next** twice then **Close**

1. [] Log on to @lab.VirtualMachine(NYCCL1).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Go to +++https://devicemanagement.microsoft.com+++ 
!IMAGE[qoyb6zad.jpg](qoyb6zad.jpg)
1. [] Select **Device Enrollment**
1. [] Under Mobile Managemet Authority, select Intune MDM Authority and click **Choose**
!IMAGE[Screenshot](Screens/zgcwig11.jpg)
1. [] Confirm that MDM is set to Intune now
!IMAGE[46258klj.jpg](46258klj.jpg)
1. [] Click on Windows Enrollment > Windows Hello for Business
!IMAGE[4km9kers.jpg](4km9kers.jpg)
1. [] Change *Configure Windows Hello for Business* to **Disabled** and click **Save**
!IMAGE[l1i7muf9.jpg](l1i7muf9.jpg)
1. Select Client apps > Apps > Click on **+Add**
!IMAGE[icsc7mr9.jpg](icsc7mr9.jpg)
1. []  On App type: Select Line-of-business app
!IMAGE[8yfmpoyx.png](8yfmpoyx.png)
1. []  On App package file > click browse and go to +++\\\\NYCCFG\\e$\\program files\\microsoft configuration manager\\bin\\i386\\ccmsetup.msi+++ then click **Open**, then click **OK**
!IMAGE[Screenshot](Screens/phjxv4im.jpg)
1. [] Click on App Information: 
	- Description: +++Configuration Manager Client+++ 
	- Publisher: +++Microsoft+++ 
	- Command Line Argument: **Copy the content from the file +++\\\\NYCCFG\\admin$\temp\\Intunecmd.txt+++**
	!IMAGE[Screenshot](Screens/sh1ub0no.jpg)
1. [] Click **OK** then click **Add**
1. [] Ignore the initial warning
!IMAGE[h4nmyekr.jpg](h4nmyekr.jpg)
1. [] Wait for the upload to complete
!IMAGE[d1939m10.jpg](d1939m10.jpg)
1. [] Select Client apps > Apps > ConfigMgr Client Setup Bootstrap
1. [] Click Assignments 
1. [] Click on Add group
1. [] In the Assignment type, select Required
1. [] Select *Included Groups*
1. [] In the Select groups to include select **Lab SCCM Install Group** and click **Select**
!IMAGE[Screenshot](Screens/gwjseoxa.jpg)
>[!ALERT] If you do not see the group in the list go to @lab.VirtualMachine(NYCCFG).SelectLink and enter the password: +++@lab.VirtualMachine(NYCCFG).Password+++, open the Command Prompt (Admin), type +++powershell+++ and press Enter, then type +++Start-ADSyncSyncCycle -PolicyType Delta+++ and press Enter

33. [] Click **OK** twice, and click **Save**
!IMAGE[Screenshot](Screens/po1s2lqz.jpg)
1. [] Go to Device Configuration
!IMAGE[r3bntiqd.jpg](r3bntiqd.jpg)
1. [] Select *Profiles*
1. [] Select **+Create Profile** and type:
	- Name: +++RootCA+++
	- Platform: select **Windows 10 and later**
	- Profile type: select **Trusted certificate**
	!IMAGE[Screenshot](Screens/qiwifeql.jpg)
1. [] Click on Settings
1. [] Under select a file, browse for  +++C:\\Users\\Administrator\ContosoRootCa.cer+++ and click **Open**, then click **OK**
!IMAGE[Screenshot](Screens/wwcanguo.jpg)

> [!KNOWLEDGE] The Root CA is needed for clients that are not AD joined, and because **For LAB purpose only** we are using an internal certificate for the CMG

39. [] Click **Create** then select Assignment
!IMAGE[Screenshot](Screens/tjvkay5r.jpg)
1. [] Click *Select groups to include*
1. [] Select **Lab SCCM Install Group**
!IMAGE[Screenshot](Screens/oofbgv4z.jpg)
1. [] Click **Select** and then **Save**
!IMAGE[Screenshot](Screens/u5vyde0x.jpg)
1. [] In a new browser tab, type +++https://portal.azure.com+++ , then go to All Services > Azure Active Directory
1. [] Select Mobility MDM and MAM
!IMAGE[Screenshot](Screens/2p4yaert.jpg)
1. [] Click on **Microsoft Intune**
!IMAGE[Screenshot](Screens/d2lgus1p.jpg)
1. [] Under *MDM User scope* select **Some**, then select **LAB SCCM Install Group**
!IMAGE[Screenshot](Screens/djqkil2h.jpg)
1. [] Click **Save**
!IMAGE[Screenshot](Screens/qy4ijisy.jpg)
1. [] Go to Azure Active Directory > Company branding > Configure
!IMAGE[p1qlrby5.png](p1qlrby5.png)
1. [] Open Paint, then click *File* > Properties and change the Image properties to +++280+++ width and +++60+++ height
!IMAGE[a7w5xbit.png](a7w5xbit.png)
and click **OK**
1. [] Click on Tools > text (select the **A**)
1. [] Type some text inside (e.g. Autopilot), go to Files > Save as > PNG Picture and save the file as +++banner.png+++ under Pictures
!IMAGE[o98485uq.png](o98485uq.png)
1. [] Repeat the steps above to create a PNG file 240x240 that you will save as +++logo.png+++
1. [] Back to the Azure portal, under *Banner logo* select Pictures > +++banner.png+++
1. [] Under Sign-in page text, type +++This is a Lab+++
1. [] Under *Square logo image* select Pictures > +++logo.png+++
1. [] Click **Save** from the top icon.


Congratulations!
You have successfully:
- Prepared Intune for Co-management

Click Next > to advance to the next exercise.
===
#Module 2, Lab 1, Exercise 4 - Co-manage a device enrolled in Azure AD

1. [] **Sign-out** @lab.VirtualMachine(NYCCL1).SelectLink 

> [!ALERT] Ensure that you have really signed out from NYCCL1

2. [] Log on using the following credentials:   
	   	- User name: Mark@yourcloudtenant.onmicrosoft.com
		- Password: +++Pa$$w0rd+++
		!IMAGE[Screenshot](Screens/d2lnffgl.jpg)

1. [] Go to All Settings > Accounts
!IMAGE[Screenshot](Screens/gdnihmhv.jpg)
1. [] Click Access work or school
!IMAGE[Screenshot](Screens/e5tiij1n.jpg)
1. [] Click Connect
1. [] Sign-in using the following credentials:   
	   	- User name: Mark@yourcloudtenant.onmicrosoft.com
		- Password: +++Pa$$w0rd+++
		!IMAGE[cprwo3r3.jpg](cprwo3r3.jpg)
1. [] At *Setting up your device* click *Got it**
!IMAGE[rukywd5f.jpg](rukywd5f.jpg)
1. [] Open *Control Panel* > *System and Security* and confirm no ConfigMgr client is installed
!IMAGE[Screenshot](Screens/nntpug2y.jpg)
1. [] Click on Work or School > Work or school account > Info
!IMAGE[u5mxhsfs.jpg](u5mxhsfs.jpg)
1. [] Click on Sync
!IMAGE[Screenshot](Screens/pw1ftxdo.jpg)

>[!NOTE] Note the information under **Applications** that indicates that via MDM the client receives the ConfigMgr bootstrap MSI

11. [] Open File Explorer and go to +++C:\\Windows+++
1. [] Double click **ccmsetup**
1. [] Go to Logs and open ccmsetup.log
1. [] Wait for the installation to complete
!IMAGE[Screenshot](Screens/m3v4ys0q.jpg)
1. [] Wait a few more minutes, then go to Control Panel > System and Security and check on the ConfigMgr client
!IMAGE[bacs3mf8.jpg](bacs3mf8.jpg)
1. [] Click the Network tab and check the Internet based Management Point
!IMAGE[Screenshot](Screens/qg11pdqe.jpg)
1. [] Open the log +++C:\\Windows\\CCM\\Logs\\CoManagementHandler.log+++ and check its content
!IMAGE[Screenshot](Screens/zp1gxv0g.jpg)

Congratulations!
You have successfully:
- Configured a Windows 10 RS3+ device to use Co-management and completed Module 2 Labs

Click Next > to advance to the next Module.

===
#Module 3, Lab 1, Exercise 1 - Capture and Test Auto Pilot

##Scenario
In this exercise you will:
- Configure Auto Pilot. 
- Upload CSV to Store for Business. 
- Create and apply Auto Pilot Profile. 
- Modern Provision Device 

1. [] Log on to @lab.VirtualMachine(NYCPR2).SelectLink using the following credentials:
	- User name: Contoso\\Administrator
	- Password: +++Pa$$w0rd+++
1. [] When you are logged in, open Windows PowerShell (Admin) 
1. [] Type +++New-VM -Name Test-autopilot -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath E:\VM\Test-autopilot.vhdx -path E:\VM -Generation 2 -SwitchName VmNAT+++ and press Enter
1. [] Type +++Set-VMProcessor Test-autopilot -Count 2 -CompatibilityForMigrationEnabled $true+++ and press Enter
1. [] Open the Hyper-V Manager console
1. [] Start "Test-autopilot" virtual machine in Hyper-V Manager. 
1. [] Right click "Test-autopilot" virtual machine in Hyper-V Manager, click "Connect..." 
1. [] Wait till the VM shows "Let's start with region. Is this right?". Press Shift+F10 key to open a command window in the VM. 
!IMAGE[ieqqoc2t.jpg](ieqqoc2t.jpg)
1. [] Type +++powershell.exe+++ and press Enter
1. [] Type +++set-executionpolicy unrestricted+++ and press Enter
1. [] Confirm with **Yes**
1. [] Type +++cd c:\autopilot+++ and press Enter
1. [] Type +++Get-WindowsAutoPilotInfo.ps1 -OutputFile c:\autopilot\testdevice.csv+++ and press Enter
1. [] Type +++net use * \\\\nyccl1.contoso.com\c$+++ and press Enter, using the following credentials:
	- User name: +++Contoso\\Administrator+++
	- Password: +++Pa$$w0rd+++
1. [] Type +++md z:\autopilot+++ and press Enter
1. [] Type +++copy c:\autopilot\testdevice.csv z:\autopilot+++ and press Enter
1. [] Log on to @lab.VirtualMachine(NYCCL1).SelectLink using the following credentials:
	- User name: Contoso\\Administrator
	- Password: +++Pa$$w0rd+++
1. [] Open Microsoft Edge or Internet Explorer and go to +++https://devicemanagement.microsoft.com+++
1. [] Log-in as Global Admin
1. [] Go to Device Enrollment > Windows Enrollment and select **Devices**
!IMAGE[6dt10xe9.jpg](6dt10xe9.jpg)
1. [] Click **Import**
!IMAGE[5dg3a3or.jpg](5dg3a3or.jpg)
1. [] Select a file > Browse to +++c:\\autopilot\\testdevice.csv+++, click **Open**
1. [] Click **Import**
!IMAGE[Screenshot](Screens/p5ldv0gh.jpg)
1. [] Monitor the Notifications
!IMAGE[9oxm6tpo.jpg](9oxm6tpo.jpg)
1. [] Once the device has been imported (it could take up to 15 minutes) 
!IMAGE[w6ns0mwy.jpg](w6ns0mwy.jpg)
1. [] Click **Sync** and then **Refresh**
!IMAGE[gghe92rz.jpg](gghe92rz.jpg)
1. [] Go to Groups > +New group
!IMAGE[5y9u2bgq.png](5y9u2bgq.png)
1. [] Select: 
	-	Group type: Security
	-	Group name: +++Autopilot+++
	-	Membership type: Assigned
1. [] At Members, add the device name that starts with a serial number, then click **Select** and click **Create**
!IMAGE[bbljck2m.png](bbljck2m.png)
1. [] Go back to Device enrollment > Windows enrollment >  Deployment Profiles
1. [] Click **+Create Profile**:
	- Name: +++Lab+++
!IMAGE[6jexuzl6.jpg](6jexuzl6.jpg)
1. [] Click *Next* for *out-of-box experience (OOBE)*:
	- Deployment mode: **User-Driven**
	- Join to Azure AD as: **Azure AD joined**
	- Microsoft Software License Terms: **Hide**
	- Privacy Settings: **Hide**
	- Hide change account option: **Hide**
	- Account Type: **Administrator**
	- Apply device name template: **No**
	!IMAGE[46zdyqbh.jpg](46zdyqbh.jpg)
1. [] Click **Next** for **Scope tags**
1. [] In **Assignments**  +Select groups to include, and select the *Autopilot* group, then click **Select**
!IMAGE[78alc90e.png](78alc90e.png)
1. [] Click **Next**. Review the information, and click **Create**
!IMAGE[cdts5pnu.png](cdts5pnu.png)
1. [] Go to Device enrollment > Windows enrollment > Devices
1. [] Click **Sync** and then **Refresh**

>[!ALERT] Do not continue until the device is showing as **Assigned** under PROFILE STATUS, it can take up to 10 minutes to complete
!IMAGE[xzd6gcr4.png](xzd6gcr4.png)

42. [] Open Windows PowerShell(admin)
1. [] Type +++cmd+++ and press Enter
1. [] Type +++cd c:\Windows\System32\Sysprep+++ and press Enter
1. [] Type +++sysprep+++ and press Enter
1. [] Select *Generalize* and press **Ok**
!IMAGE[92tz3v9d.jpg](92tz3v9d.jpg)
1. [] After several minutes, the machine will restart and go back to OOBE.
1. [] On the *Let's start with region. is this right?*  Select the appropriate region and click **Yes.**
1. [] At *is this the right keyboard layout?* Click **Yes**
1. [] On the want to add a second keyboard layout, click **Skip**
1. [] Wait for the process to continue
!IMAGE[rpx4t7pk.jpg](rpx4t7pk.jpg)
1. [] On the network screen it may time some time, just wait. Note: The Machine may restart if a new update is found. 
1. [] At *Sign in with Microsoft* enter Mark UPN, example mark@lode1234567.onmicrosoft.com

> [!NOTE] Note the different logon screen, with the Welcome to Contoso! this is a simple indicator that the machine has been recognized by the Autopilot service
!IMAGE[jlz1m73y.png](jlz1m73y.png)

54. [] Type +++@lab.VirtualMachine(NYCPR2).Password+++ as password and click **Next**
1. [] Wait until is completed, during this time Autopilot will proceed with the rest of the information
1. [] Logon as Mark then go to All Settings > Accounts> Access work or school > Click on Connected to Contoso's Azure AD > Info
!IMAGE[j3s7ip00.jpg](j3s7ip00.jpg)
1. [] On the Managed By Contoso, click **Sync** under Device sync status
1. [] Then Open File Explorer and go to Local Disk (C:) > Windows > Once the policy is downloaded you will see a ccmsetup folder created, and in a few more minute the CCM folder will appear as well.
!IMAGE[4hohclih.png](4hohclih.png)
1. [] Go to Control Panel 
1. [] Open the Configuration Manager GUI and note that the certificate is self-signed but the connection type is Always Internet
!IMAGE[ldgg6obt.jpg](ldgg6obt.jpg)

> [!NOTE] It would take several more minutes before all the policies are downloaded because the Microsoft Store is updating all Windows Apps in the background, using pretty much all the available Internet connection.
!IMAGE[1aqdakw1.jpg](1aqdakw1.jpg)
> [!NOTE] and finally, the ConfigMgr will receive all the policies as expected
!IMAGE[luxs40u4.jpg](luxs40u4.jpg)

Congratulations! 
You have successfully:

- Configure Auto Pilot.
- Upload CSV to Intune.
- Create and apply Auto Pilot Profile.
- Modern Provision Device

Click Next > to advance to the next exercise.
===
#Module 3, Lab 1, Exercise 2 - Explore Co-Management Dashboard and Reporting

##Scenario
In this exercise you will:
- Explore Dashboard for Co-Management

1. [] Log on to @lab.VirtualMachine(NYCCFG).SelectLink using the following credentials:   
	   	- User name: Contoso\\Administrator  
		- Password: +++Pa$$w0rd+++
1. [] Open Configuration Manager Console and click on Monitoring Workspaces
1. [] Click on Co-management node
1. []  Co-Management Dashboard will be displayed. If there is no data. wait until the summarization is completed.
!IMAGE[tbn2vg59.jpg](tbn2vg59.jpg)
1. [] Click on Cloud Management node and review the information
!IMAGE[dvhs74dz.jpg](dvhs74dz.jpg)

Congratulations! 

You have successfully: 

- Explored the Dashboard for Co-Management.

Looks like this is the last Exercise, this is not a level 200 lab. So, we want to congratulate you in finishing this amazing lab.