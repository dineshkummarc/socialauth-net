## SocialAuth.NET + Security Token Service (STS) ##

http://socialauth-net.googlecode.com/svn/wiki/images/sts_process_flow.PNG

**STS is a form of web endpoint which facilitates delegation of authentication requirements to a separate centralized setup. Extracting security framework and delegating it to STS gives following benefits:**

  * Centralized authentication endpoint for multiple applications
  * Communication being done using standard web protocols, STS can be utilized by applications of varied platforms
  * Easy maintenance with  single-point user management

STS in .NET can be easily implemented with minimal code requirements through Windows Identity Foundation. WIF provides both STS provider and STS client project types with some boilerplate code. All this facilitates in creating STS consumer and STS provider applications in minutes. On top of it, WIF API and normal server side programming languages like C# can be used to program advance security frameworks as per business needs.

Authentication through WIF STS provider is supported by SharePoint, Azure, MVC and general ASP.NET applications.

SocialAuth.NET can be integrated with a WIF STS provider. This can help in implementing ideas like enabling access to a SharePoint site via Facebook etc.

Following is a general process flow depicting how various components wire up to support creation of sophisticated security frameworks.

http://socialauth-net.googlecode.com/svn/wiki/images/sts_process_detailedflow.PNG



1.	User tries to access secured resource of relying party (RP)

2.	RP initiates a request to STS Website and redirects user to it for authentication

3.	STS website displays option to connect with different OAuth providers using SocialAuth.NET

4.	User selects a provider

> a.	SocialAuth.NET initiates a OAuth handshake with provider

> b.	User is redirected to selected provider for login

> c.	Upon successful login, provider redirects user back to STS with access token

5.	STS uses SocialAuth.NET library to fetch user details like first name, profile picture etc.

6.	STS wraps user details in SAML tokens

7.	User is redirected back to relying party along with SAML tokens

8.	WIF at relying party validates SAML tokens and grants access to requested resource



## Pre-requisites ##
1.	[Download](http://www.microsoft.com/download/en/details.aspx?id=17331) & install WIF Runtime

2.	 [Download](http://www.microsoft.com/download/en/details.aspx?id=4451#overview) & Install WIF SDK for Visual Studio


3.       [Download](http://code.google.com/p/socialauth-net/downloads/list) and unzip SocialAuth.NET package (Download 2.2 or above version)

4.	 (Optional) [Download](http://socialauth-net.googlecode.com/files/SocialAuth_STS_Installer.zip) SocialAuth.NET STS Installer

5.	Visual Studio 2010

6.	IIS 7


## Tutorials ##
Please ensure all pre-requisites have been met.

1.	How to create simple SocialAuth.NET STS service provider ([view](tutorial_creating_sts.md))

2.	How to integrate ASP.NET application as a consumer with SocialAuth.NET STS ([view](tutorial_sts_aspnet_consumer.md))

3.	How to integrate SharePoint as a consumer with SocialAuth.NET STS ([view](tutorial_sts_sharepoint_consumer.md))

4. How to setup STS using SocialAuth.NET STS Installer ([view](http://code.google.com/p/socialauth-net/wiki/tutorial_creating_sts#Setting_up_SocialAuth_.NET_STS__using_Installer))