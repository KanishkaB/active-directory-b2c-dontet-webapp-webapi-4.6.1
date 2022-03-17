# active-directory-b2c-dontet-webapp-webapi-4.6.1
 
page_type	description	languages	products
sample
The sample covers calling an OpenID Connect identity provider (Azure AD B2C) and acquiring a token from Azure AD B2C using MSAL.
csharp
dotnet
azure
azure-active-directory
Azure AD B2C: Call an ASP.NET Web API from an ASP.NET Web App
This sample contains a solution file that contains two projects: TaskWebApp and TaskService.

TaskWebApp is a "To-do" ASP.NET MVC web application where the users enters or updates their to-do items. These CRUD operations are performed by a backend web API. The web app displays the information returned from the ASP.NET Web API.
TaskService is the backend ASP.NET API that manages and stores each user's to-do list.
The sample covers the following:

Calling an OpenID Connect identity provider (Azure AD B2C)
Acquiring a token from Azure AD B2C using MSAL
How To Run This Sample using your own Azure AD B2C Tenant
After cloning this repo, configure the sample to use your own Azure AD B2C tenant. In this section, you'll learn how to configure the ASP.NET Web Application and the ASP.NET Web API to work with your own Azure AD B2C Tenant.

Step 1: Get your own Azure AD B2C tenant
First, you'll need an Azure AD B2C tenant. If you don't have an existing Azure AD B2C tenant that you can use for testing purposes, you can create your own by following these instructions.

Step 2: Create your own policies
This sample uses three types of policies: a unified sign-up/sign-in policy, a profile editing policy, and a password reset policy. Create one policy of each type by following the built-in policy instructions. You may choose to include as many or as few identity providers as you wish.

If you already have existing policies in your Azure AD B2C tenant, feel free to re-use those policies in this sample.

Make sure that all the three policies return User's Object ID and Display Name on Application Claims. To do that, on Azure Portal, go to your B2C Directory then click User flows (policies) on the left menu and select your policy. Then click on Application claims and make sure that User's Object ID and Display Name is checked.

Step 3: Register your ASP.NET Web API with Azure AD B2C
Follow the instructions at register a Web API with Azure AD B2C to register the ASP.NET Web API sample with your tenant. Registering your Web API allows you to define the scopes that your ASP.NET Web Application will request access tokens for.

Provide the following values for the ASP.NET Web API registration:

Provide a descriptive Name for the ASP.NET Web API, for example, My Test ASP.NET Web API. You will identify this application by its Name whenever working in the Azure portal.
Set the Reply URL to https://localhost:44332/. This is the port number that this ASP.NET Web API sample is configured to run on.
Set the AppID URI to demoapi. This AppID URI is a unique identifier representing this particular ASP.NET Web API. The AppID URI is used to construct the scopes that are configured in your ASP.NET Web Application. For example, in this ASP.NET Web API sample, the scope will have the value https://<your-tenant-name>.onmicrosoft.com/demoapi/read
Create the application.
Once the application is created, open your My Test ASP.NET Web API application and then open the Published Scopes window (in the left nav menu). Add the following 2 scopes:
Scope named read followed by a description demoing a read scenario.
Scope named write followed by a description demoing a write scenario.
Click Save.
Step 4: Register your ASP.NET Web Application with Azure AD B2C
Follow the instructions at register a Web Application with Azure AD B2C

Your web application registration should include the following information:

Provide a descriptive Name for your web application, for example, My Test ASP.NET Web Application. You can identify this application by its Name within the Azure portal.
Set the Reply URL to https://localhost:44316/ This is the port number that this ASP.NET Web Application sample is configured to run on.
Create your application.
Once the application is created, from the menu select Authentication. In the Implicit grant section, select Access tokens.
Next, create a Web App client secret. In the Azure portal go to your Azure AD B2C instance. From the menu select App registration. Select the registration for your Web Application. From the menu select Certificates & secrets and click New client secret. Note: You will only see the secret once. Make sure you copy it.
From the menu choose API permissions. Click Add a permission, switch to the My APIs tab, and select the name of the Web API you registered previously, for example My Test ASP.NET Web API. Select the scope(s) you defined previously, for example, read and write and select Add permissions.
Step 5: Configure your Visual Studio project with your Azure AD B2C app registrations
In this section, you will change the code in both projects to use your tenant.

⚠️ Since both projects have a Web.config file, pay close attention which Web.config file you are modifying.

Step 5a: Modify the TaskWebApp project
Open the Web.config file for the TaskWebApp project.

Find the key ida:Tenant and replace the value with your <your-tenant-name>.onmicrosoft.com.

Find the key ida:AadInstance and replace the value with your <your-tenant-name>.b2clogin.com.

Find the key ida:TenantId and replace the value with your Directory ID. You can get it by navigating to the registration information of one of your apps and copying the value of the Directory (tenant) ID property.

Find the key ida:ClientId and replace the value with the Application ID from your web application My Test ASP.NET Web Application registration in the Azure portal.

Find the key ida:ClientSecret and replace the value with the Client secret from your web application in the Azure portal.

Find the keys representing the policies, e.g. ida:SignUpSignInPolicyId and replace the values with the corresponding policy names you created, e.g. b2c_1_SiUpIn

Change the api:ApiIdentifier key value to the App ID URI of the API you specified in the Web API registration. This App ID URI tells B2C which API your Web Application wants permissions to.

<!--<add key="api:ApiIdentifier" value="https://tenant.onmicrosoft.com/api/" />—>

<add key="api:ApiIdentifier" value="https://<your-tenant-name>.onmicrosoft.com/demoapi/" />
📝 Make sure to include the trailing '/' at the end of your ApiIdentifier value.

Find the keys representing the scopes, e.g. api:ReadScope and replace the values with the corresponding scope names you created, e.g. read

Step 5b: Modify the TaskService project
Open the Web.config file for the TaskService project.
Find the key ida:Tenant and replace the value with your <your-tenant-name>.onmicrosoft.com.
Find the key ida:AadInstance and replace the value with your <your-tenant-name>.b2clogin.com.
Find the key ida:ClientId and replace the value with the Application ID from your web API My Test ASP.NET Web API registration in the Azure portal.
Find the key ida:SignUpSignInPolicyId and replace the value with the policy name you created, e.g. b2c_1_SiUpIn
Find the keys representing the scopes, e.g. api:ReadScope and api:WriteScope and replace the values with the corresponding scope names you created if needed, e.g. read and write
Step 5c: Run both projects
You need to run both projects at the same time. If you did not complete the demo tenant instructions above, you need to configure Visual Studio for multiple startup projects.

You can now perform all the previous steps as seen in the demo tenant environment.

Known Issues
MSAL cache needs a TenantId along with the user's ObjectId to function. It retrieves these two from the claims returned in the id_token. As TenantId is not guaranteed to be present in id_tokens issued by B2C unless the steps listed in this document, if you are following the workarounds listed in the doc and tenantId claim (tid) is available in the user's token, then please change the code in ClaimsPrincipalsExtension.cs GetB2CMsalAccountId() to let MSAL pick this from the claims instead
