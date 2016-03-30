# Tutorial - Setting up STS Service #
This tutorial explains how you can quickly setup up a Security Token Service (STS) provider using Windows Identity Foundation and integrate SocialAuth.NET into it. STS once up and running can be utilized by any consumer like a SharePoint or a WIF enabled ASP.NET website. STS can be setup manually by performing a series of steps or by using SocialAuth.NET STS installer which automatically manages most of the manual process. We’ll follow both the approaches in this tutorial.

## Manually setting up SocialAuth.NET STS ##

**Steps:**

1.	Open Visual Studio 2010 and select **File > New Website**

2.	From the templates option, select **ASP.NET Security Token Service Website** (This option would appear only if you have installed WIF SDK for Visual Studio). Specify path and create a new website called “SocialAuthSTS” .


http://socialauth-net.googlecode.com/svn/wiki/images/createproject.PNG


Visual Studio will create project with following Folder Structure

http://socialauth-net.googlecode.com/svn/wiki/images/provider_structure.PNG


With this step done, your site is ready to be hosted as an STS. It has some boilerplate code to allow forms based authentication through login.aspx web form.


http://socialauth-net.googlecode.com/svn/wiki/images/login.PNG

(Login.aspx screenshot)


You can test this application by creating a STS consumer website (Refer to creating an ASP.NET STS consumer tutorial).
Our intent however is to use SocialAuth.NET instead of this default demo login form so that user need not enter credentials but login via providers like Facebook. Hence we’ll replace some boilerplate HTML and code, to suit our requirement.

3.	Now we begin with steps to integrate SocialAuth.NET to this STS website

**Add references** to all DLLs required for SocialAuth.NET (These DLLs can be found inside Bin folder of unzipped SocialAuth.NET package download. Refer to pre-requisites for download link)

  * SocialAuth-net.DLL
  * Newtonsoft.Json.dll
  * !Log4net.dll (This is an optional dll. Refer it only if you want to enable Log4Net logging framework)

In general, you also need to add Microsoft.IdentityModel.dll. However, it is automatically added by default when creating STS website project.

4.	Open Web.Config perform following changes:

a.	Add following under `<Configuration>` Element
```
<configSections>
    <section name="SocialAuthConfiguration" type="Brickred.SocialAuth.NET.Core.SocialAuthConfiguration, SocialAuth-net, Version=2.1.0.0, Culture=neutral, PublicKeyToken=null" allowDefinition="Everywhere" allowLocation="true" />
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler" />
  </configSections>
  <!-- Social Auth Configuration-->
  <SocialAuthConfiguration>
    <Providers>
      <add  WrapperName="FacebookWrapper" ConsumerKey="152190004803645" ConsumerSecret="64c94bd02180b0ade85889b44b2ba7c4"/>
      <add  WrapperName="TwitterWrapper" ConsumerKey="E3hm7J9IQbWLijpiQG7W8Q" ConsumerSecret="SGKNuXyybt0iDdgsuzVbFHOaemV7V6pr0wKwbaT2MH0" />
    </Providers>
   <IconFolder Path="~/images/SocialAuthIcons/" />
     </SocialAuthConfiguration>
```
Here, we have added SocialAuth.NET configuration settings like providers supported, their keys and secret and a few other details. If you wish to use your own keys/secret, modify aforesaid settings accordingly. For more details on configuration, you may visit [Web.Config Anatomy Wiki](WebConfig_Reference.md).

b.	To register required handlers and modules, add following inside <System.Web> element:
```
<httpHandlers>
      <add verb="*" path="*.sauth" type="Brickred.SocialAuth.NET.Core.CallbackHandler" />
    </httpHandlers>
    <httpModules>
      <add name="SocialAuthAuthentication" type="Brickred.SocialAuth.NET.Core.SocialAuthHttpModule" />
    </httpModules>
```
c.	Add following after </System.Web> to register handlers and modules for IIS7
```
<system.webServer>
    <validation validateIntegratedModeConfiguration="false" />
    <handlers>
      <add name="SocialAuth.NET" verb="*" path="*.sauth" type="Brickred.SocialAuth.NET.Core.CallbackHandler" />
    </handlers>
    <modules>
      <add name="SocialAuthAuthentication" type="Brickred.SocialAuth.NET.Core.SocialAuthHttpModule" />
    </modules>
  </system.webServer>
```

d. Application by default uses FormsAuthentication. To ensure SocialAuth.NET is able to run its routines, add following in Web.Config
```
 <location path="socialauth">
    <system.web>
      <authorization>
        <allow users="*"/>
      </authorization>
    </system.web>
  </location>
```

You can add this configuration block before '<System.Web>'.

5.	SocialAuth.NET is integrated to project. Now add some UI elements to allow user to login through SocialAuth.NET.

a.	Open Login.aspx  (HTML source) and remove everything except <@Page/> tag

b.	Add following HTML under page tag and save:
```
<html xmlns="http://www.w3.org/1999/xhtml">
<head id="Head1" runat="server">
    <title>Login to the security token service (STS)</title>
</head>
<body>
    <h1>SocialAuth.NET STS demo</h1>
    <form id="form1" runat="server">
    <asp:Button ID="btnFacebook" Text="Login With Facebook" OnClick="btnFacebook_Click" runat="server" />
    <asp:Button ID="btnTwitter" Text="Login with Twitter" OnClick="btnTwitter_Click" runat="server" />
    </form>
</body>
</html>
	
```

This simple UI has 2 buttons that allow user to login with Facebook or Twitter.

c.	Open Login.aspx.cs and replace code with following:
```

using System;
using System.Web.Security;
using Brickred.SocialAuth.NET.Core.BusinessObjects;
 
public partial class Login : System.Web.UI.Page
{
 
    protected void Page_Load( object sender, EventArgs e )
    {
    }  
  
    protected void btnFacebook_Click(object sender, EventArgs e)
    {
        SocialAuthUser.GetCurrentUser().Login(PROVIDER_TYPE.FACEBOOK);
    }
 
 
    protected void btnTwitter_Click(object sender, EventArgs e)
    {
        SocialAuthUser.GetCurrentUser().Login(PROVIDER_TYPE.TWITTER);
    }
}
```

Compile your project! It should compile successfully.

This code is simply handles button\_click event and initiates login with respective provider. When user successfully logs into Facebook/Twitter, they are redirected back to Default.aspx. As a next step, Our STS should convert user profile information into SAML tokens and Redirect user back to STS Consumer Website.

No changes are required for redirection as that is automatically managed by WIF, while following code changes creates SAML tokens.

6.	Open App\_Code -> CustomSecutiryTokenService.cs

**GetOutputClaimsIdentity** method processes SAML tokens. Replace default code to create SAML tokens as following.
```
 protected override IClaimsIdentity GetOutputClaimsIdentity(IClaimsPrincipal principal, RequestSecurityToken request, Scope scope)
    {
        ClaimsIdentity outputIdentity = new ClaimsIdentity();
        if (null == principal)
        {
            throw new ArgumentNullException("principal");
        }
        if (SocialAuthUser.IsLoggedIn())
        {
            List<Claim> userClaims = new List<Claim>();
            UserProfile profile = SocialAuthUser.GetCurrentUser().GetProfile();
            outputIdentity.Claims.Add(new Claim(ClaimTypes.Authentication, profile.Provider.ToString()));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.EmailAddress, string.IsNullOrEmpty(profile.Email) ? "emailNotKnown" : profile.Email));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.Role, "socialauth"));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.DateOfBirth, string.IsNullOrEmpty(profile.DateOfBirth) ? "" : profile.DateOfBirth.ToString()));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.Upn, profile.ID.ToString()));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.Uri, profile.ProfilePictureURL));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.Gender, profile.Gender));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.Name, profile.FullName));
            outputIdentity.Claims.Add(new Claim(ClaimTypes.DefaultName, profile.FullName));
        }
 
        return outputIdentity;
    }

```

In our implementation of **GetOutputClaimsIdentity** , we're simply extracting values from SocialAuth.NET's User Profile object, and adding them as claims using WIF's method to add claims (These claims are automatically converted to SAML tokens by WIF).


Also, append following class in same file.
```
public class ClaimTypes
{
    // System.IdentityModel.Claims.ClaimTypes.Email;
    public static string EmailAddress =
            "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress";
    public static string Authentication = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication";
    public const string DateOfBirth = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/dateofbirth";
    public const string Gender = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender";
    public const string Name = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
    public const string Role = "http://schemas.microsoft.com/ws/2008/06/identity/claims/role";
    public const string Upn = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn";
    public const string Uri = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/uri";
    public const string DefaultName = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname";
 
}
```
This class is just a utility class containg namespace for different types of claims. It is used when adding claims like following:
```
 outputIdentity.Claims.Add(new Claim(ClaimTypes.Gender, profile.Gender));
```

You'd also need to add 2 namespaces so that code compiles properly.
```
using System.Collections.Generic;
using Brickred.SocialAuth.NET.Core.BusinessObjects;
```

7.	We also need to make few changes to Default.aspx.cs. It is this page which executes WIF code to initiate aforesaid processing of claims and redirection of user back to relying party.

Open Default.aspx and add find following conditional block:
```
	else if ( action == WSFederationConstants.Actions.SignOut )
            {
                // Process signout request.
                SignOutRequestMessage requestMessage = (SignOutRequestMessage)WSFederationMessage.CreateFromUri( Request.Url );
                FederatedPassiveSecurityTokenServiceOperations.ProcessSignOutRequest( requestMessage, User, requestMessage.Reply, Response );
            }
```

Copy following additional else-if conditional block just after aforesaid block (before final "else").
```
            else if (action == null && SocialAuthUser.IsLoggedIn())
            {
                string originalUrl = SocialAuthUser.GetCurrentUser().GetConnection(SocialAuthUser.CurrentProvider).GetConnectionToken().UserReturnURL;
                SignInRequestMessage requestMessage = (SignInRequestMessage)WSFederationMessage.CreateFromUri(new Uri(originalUrl));
                if (User != null && User.Identity != null && User.Identity.IsAuthenticated)
                {
                    SecurityTokenService sts = new CustomSecurityTokenService(CustomSecurityTokenServiceConfiguration.Current);
                    SignInResponseMessage responseMessage = FederatedPassiveSecurityTokenServiceOperations.ProcessSignInRequest(requestMessage, User, sts);
                    FederatedPassiveSecurityTokenServiceOperations.ProcessSignInResponse(responseMessage, Response);
                }

            }
```


You would need to include following on top:
```
using Brickred.SocialAuth.NET.Core.BusinessObjects;
```


Compile your code! It should compile successfully.



8.	**Finally** we need to host this application in IIS.

> a. Open IIS7 (Shortcut: Open Run -> type inetmgr and press enter)

> b.	Host application in IIS by creating a application pointing to application under default website. Right Click **Default Website** and select **Add Application**.

> http://socialauth-net.googlecode.com/svn/wiki/images/addapplication.PNG

> Specify Physical Path as location where you've created STS website along with an Alias. If you're strictly following tutorial, path should be **c:\temp\SocialAuthSTS**.

> http://socialauth-net.googlecode.com/svn/wiki/images/specifywebsitepath.PNG


> c.	Open hosts file (C:\Windows\System32\drivers\etc) in notepad and append following at the end:
```
     127.0.0.1	opensource.brickred.com
```

> If you are using your keys/secrets, please specify your registered application domain instead of opensource.brickred.com.

> Run your application as http://opensource.brickred.com/SocialAuthSTS/login.aspx (use your domain if using different keys)

> You should see login.aspx as following:

> http://socialauth-net.googlecode.com/svn/wiki/images/sts_login.PNG

> Move to What's next section at bottom of this page.

## Setting up SocialAuth.NET STS  using Installer ##

If you wish a quicker way to setup STS, we have SocialAuth.NET STS 	Installer for you. It primarily does following:

  * Creates a virtual directory under your specified website and copies sample website application
  * Allows you to specify custom keys/secrets along with scope or simply configure application using opensource.brickred.com demo environment
  * Once you have installed application, you simply need to change the design of Login.aspx to suit your needs

> Following is a step by step instructuin on how you can use SocialAuth.NET STS Installer:-

  1. Run Installer. This will open following wizard in which click "Next"

> http://socialauth-net.googlecode.com/svn/wiki/images/socialauth_sts_installer1.PNG

> 2. Second screen of installer is where you can
    * Specify website under which this application will be installed (If you do not wish to use Default web site, you need to create one through IIS)
    * Specify Name for Virtual Directory. For example, if you specift virtual directory as SocialAuthSTS under somewebsite.com, Your application will be accessible as somewebsite.com/SocialAuthSTS.
    * You can also explicitly specify App Pool you wish to use with this application.
> > If you are not clear of aforesaid IIS settings, you may simply click "Next". This would create a new virtual directory under Default Website and will copy application under C:\inetpub\wwwroot.


> http://socialauth-net.googlecode.com/svn/wiki/images/socialauth_sts_installer2.PNG

> 3.  In Confirmation Installation wizard screen, click Next. This would initiate installation of site.

#### SocialAuth.NET settings ####

> 4. Next screen in this wizard presents configuration option.

> http://socialauth-net.googlecode.com/svn/wiki/images/socialauth_sts_installer3.PNG

> If you wish to use your own application registered with one or more providers, you may opt for option 1. This option assumes that you have fair idea of SocialAuth.NET and would be able to specify keys, secrets and scopes. When using this option, simply select the providers you wish to integrate your STS with and click Next.

> If you wish to try a demo of SocialAuth.NET STS, opt for 2nd option. This option uses keys, secrets and default scopes as specified for opensource.dashboard.com, which is used as a demo application for SocialAuth.NET. When using this option, you do not need to specify any other setting and simply select "Next". When using this option, skip step 5. In order to be able to run opensource.brickred.com from your machine, installer makes an entry in SystemDrive/System32/Drivers/etc/hosts file.

> 5. When using custom mode of installation, installer next presents provider configuration screens for all selected providers individually as shown below:

> http://socialauth-net.googlecode.com/svn/wiki/images/socialauth_sts_installer4.PNG

> You need to specify following:

  * Consumer Key (or Application ID) for the registered application
  * Consumer Secret as provided by OAuth provider
  * Scopes to be used. Either you may choose "default" which connects with provider using basic scopes of getting profile and friends. Instead you may select "custom" which provides you option to specify scopes explicitly. When using custom, no scope is added by default. You can specify multiple scopes using the delimeter as accepted by provider. For example, most providers accept comma (,) which a few accept only space( ).

> This step will repeat itself for all selected providers.

> 6. Installer ill display "Installation Complete" screen. Click "close".

> http://socialauth-net.googlecode.com/svn/wiki/images/socialauth_sts_installer5.PNG

> Try to run your STS using http://YourHost/Virtual Directory Alias/Login.aspx. If you opted to set up demo environment your sts demo link would be
> http://opensource.brickred.com/SocialAuthSTS/Login.aspx and would should display as following

> http://socialauth-net.googlecode.com/svn/wiki/images/socialauth_sts_installer6.PNG

> ## What's Next ##
> Your STS installer is ready.

> Try clicking on Twitter/Facebook button. If you don't get redirected to their login form, there is a mismatch between keys and domain registered with them and should be resolved. You should not browse your application using http://localhost.

We are through with setting up of STS and now it can serve as an authentication provider for any STS consumer. You can navigate to directory where STS is installed and modify Web.Config with any key, secret, scope or geeneral SocialAuth.net settings.

Following are links to tutorials for creating consumer application:

  * [Create simple ASP.NET consumer](tutorial_sts_aspnet_consumer.md)
  * [Use STS service using SharePoint as consumer](tutorial_sts_sharepoint_consumer.md)