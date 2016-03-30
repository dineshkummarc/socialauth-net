## 1. New Providers added ##

SocialAuth.NET 2.0 now supports 3 more providers - Twitter, MySpace and LinkedIn besides Yahoo, Google, Facebook and MSN.
![http://socialauth-net.googlecode.com/svn/wiki/images/supported_providers.png](http://socialauth-net.googlecode.com/svn/wiki/images/supported_providers.png)

## 2. Multiple Connections ##

SocialAuth.NET 2.0 allows simultaneous connections to multiple providers where as in 1.x every new connection overrides previous one. User control is also updated accordingly to highlight selected connections.
Across the documentation, "Current Connection" refers to the last connected connection.

## 3. Revamped Architecture ##

SocialAuth.NET is now specifications oriented rather than provider oriented (as was the case in 1.x). SocialAuth.NET 2.0 supports authentication flows of
  * OAuth1.0a
  * OAuth2.0
  * OpenID+OAuth2.0 Hybrid protocols. If you want to add any new provider supporting any of these specifications, it is far easier than 1.x now.

Our 7 supported providers are implemented as following:
  1. Facebook - OAuth2.0
  1. MSN - OAuth 2.0
  1. Yahoo - OAuth+OpenID Hybrid
  1. Google - OAuth+OpenID Hybrid
  1. Twitter - OAuth1.0a
  1. MySpace - OAuth1.0a
  1. LinkedIn - OAuth1.0a

## 4. Custom Feed Execution ##

SocialAuth.NET 2.0 continues to provide profile and contacts as in 1.x. However, till we add more standard features, you can leverage power of SocialAuth.NET by executing custom REST API for supported providers. For example, you can get likes from Facebook or calendar entries from Google. Hence, you are now not limited to Contacts and Profile only.

## 5. Custom Scope ##

SocialAuth.NET 1.x required user permission for all supported features. However, what if you just want to authenticate and do not need contacts? Such customization was not possible earlier. However, SocialAuth.NET 2.0 brings a new optional attribute "ScopeLevel" which can be set to Custom or Default.
```
<add "ScopeLevel="CUSTOM" WrapperName="MSNWrapper" . . .
<add "ScopeLevel="DEFAULT" WrapperName="MSNWrapper" . . .
```
Use "Custom", when you **do not want** SocialAuth to add any scope automatically. Use "Default" when you wish SocialAuth to automatically add all scopes that are required for functioning of default features - Contact & Profile.

Irrespective of any ScopeLevel, you can also provide additional scopes in optional "AdditionalScopes" attribute. For example, if you do not want any default scope for Google but want to access Calendar, your tag will look like:
```
<add ScopeLevel="CUSTOM" WrapperName="GoogleWrapper" ConsumerKey="Your Key" ConsumerKey="YourSecret" AdditionalScopes="https://www.google.com/calendar/feeds/"/>
```
If ScopeLevel is not defined, SocialAuth.NET with work with default scopes with DEFAULT behavior.

## 6. Base Domain ##

If you do not want SocialAuth.NET to automatically map folder directory in IIS for redirection purpose, you can specifically provide a domain in Web.config under SocialAuthConfiguration as:
```
<BaseURL Domain="mysite.com"/>
```
If this tag is not specified, SocialAuth.NET works as usual.

## 7. Login Callback ##

If you want to hook any method after authentication is successful and SocialAuth.NET is about to direct user to default URL, you can now provide a callback when calling Login Method. This method is useful is you wanted to redirect user to somewhere else or if you need to set any session variable or execute any logic before user knows that he has logged in.
```
SocialAuthUser.GetCurrentUser().Login(PROVIDER_TYPE.FACEBOOK,DoSomething)

public static void DoSomething()
{
	//some code
}
```

## 8. Detailed Log and Better Exceptions ##

If you are using Log4NET, you will find much more detailed logs now. Also, exceptions will now be more specific categorized into 3 forms -

**OauthFlowException**: When an error occurs while execution of OAuth process
**DataParsingException**: When an error occurs while parsing profile or contacts
**InvalidSocialAuthConnectionException**: When there is an attempt to do something with provider, user is not connected to.
Any error not falling in any of above category will be thrown as it is.

## 9. Redirect to requested page like Forms Authentication ##
In 1.x version, if a user requested any page without logging in to any provider, he was redirected to login url and after login with any provider he was always taken back to default uul. In 2.0, user will be redirected to original page he requested.

## 10. Access Token ##

You can now access Access Token using :
```
SocialAuthUser.GetCurrentUser().AccessToken                 
```