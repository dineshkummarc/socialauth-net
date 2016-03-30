# Introduction #

This page hihglights an important Update on Google integration and also helps you troubleshoot Invalid Token Received error.

# Details #

There are 2 ways to register your Application with Google:
<h3> 1. Using Hybrid Model (OpenID + OAuth1.0)</h3>
  * You're using this model, if you've registered your application from http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html
  * SocialAuth.Net has been supporting this version only till 2.3.1a
  * You get a ConsumerKey here (and not clientID)

<h3> 2. Using OAuth2.0 </h3>
  * You're using this model if you've registered your application from https://code.google.com/apis/console
  * Support for this has been incorporated since version 2.4
  * You get a ClientID here. Something like 23123123.apps.googleusercontent.com


<h3>What happens in Hybrid Mode?</h3>

When Google is connected in Hybrid Mode, irrespective of the consumer key/secret, it authenticates the user and returns several fields - First Name, Last Name, EmailID etc. However, for any further interaction, a valid OAuth token is needed which is provided only when you're using the correct Keys/Secret/OAuth Version.

SocialAuth.Net! default mode for Google till version 2.3.1a has been Hybrid ( OAuth2.0 isn't supported for versions before 2.4). Hence, if you've registered application using 2nd option and using Default Scope (i.e. You want Authentication, Profile and Friends), SocialAuth.Net throws Invalid Request Token error which is because Google doesn't returns OAuth token (required for Profile and Contacts) and just returns Authenticated user.

Now with 2.4, you've the option to continue with Hybrid or use OAuth2.0 with Google.

<h3> What I need to change? </h3>

If you're using OAuth1.0 hybrid, you need to use GoogleHybridWrapper
> 

&lt;add WrapperName="GoogleHybridWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;



Else, if you're using OAuth2.0, then you need GoogleWrapper
> 

&lt;add WrapperName="GoogleWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;



**IMPORTANT NOTE**
If you've upgraded from a previous version and have been using Google Wrapper, you'd need to change the name of wrapper from GoogleWrapper to GoogleHybridWrapper in your config as we've upgraded Google Wrapper to use their OAuth 2.0 flow**. However, previous OAuth 1.0 Hybrid is still there which you can use by replacing GoogleWrapper to GoogleHybridWrapper if you're upgrading your library to 2.4**<br>

<h3>If I'm still getting Error?</h3>
<ul><li>If you're doing a copy paste of key from browser to config, ensure that there are no spaces copied at beginning/end of consumer key.<br>
</li><li>URL registered and matched should exactly match what's there in config. For example, if it is www.site.com the same should be in config (and likely in same case). Putting <a href='http://site.com'>http://site.com</a> OR <a href='http://www.site.com'>http://www.site.com</a> or site.com may result in such exception<br>
</li><li>Please register separate key for sub domains i.e. if you have registered www.somesite.com, it can not be used for www.sub.somesite.com. A separate key/secret should be generated for later.