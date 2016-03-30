![http://socialauth-net.googlecode.com/svn/wiki/images/banner.png](http://socialauth-net.googlecode.com/svn/wiki/images/banner.png)


### Release Update ###

#### SocialAuth.Net 2.4.1 released on 8/May/2014  [download](https://code.google.com/p/socialauth-net/source/browse/Releases/2.4.1.rar) ####
<b>Issues Fixed in v2.4.1.0</b>
<pre>
- Fixed Issue 199: Twitter endpoints updated (Twitter Blocker)<br>
- Issue with getting profile for Yahoo fixed<br>
- Issue with parsing profile for Google fixed<br>
- In case of windows authentication, explicit returnUrl wasn't respected. This is fixed.<br>
- Fixed Issue 195: https in Google contacts endpoint<br>
- Fixed Issue 198: MSN profile issue when image is null</pre>

<br />

![http://socialauth-net.googlecode.com/svn/wiki/images/nuget.png](http://socialauth-net.googlecode.com/svn/wiki/images/nuget.png)<br><br>

Facing trouble in trying demo application? Checkout following video!<br>
<a href='http://www.youtube.com/watch?feature=player_embedded&v=FOaUAKaSg0k' target='_blank'><img src='http://img.youtube.com/vi/FOaUAKaSg0k/0.jpg' width='425' height=344 /></a><br>
<br>
<i><a href='http://code.google.com/p/socialauth-net/issues/entry'>Suggest</a> a feature!</i>
<br>

<a href='http://labs.3pillarglobal.com/socialauthdemo-net/default.aspx'><img src='http://socialauth-net.googlecode.com/svn/wiki/images/btn-try-demo.png' /></a>

<h1>What is SocialAuth.NET?</h1>
SocialAuth.NET is a .NET port of popular <a href='http://code.google.com/p/socialauth/'>Java based SocialAuth library</a>. Written from ground up using C#.NET, this component seamlessly integrates into any ASP.NET web application and enables user authentication through different service providers with minimal development effort. SocialAuth.NET hides all the intricacies of generating signatures & token, doing security handshakes and provide out of the blue simple API to interact with providers.<br>
<br>
With this library you can:<br>


<ul><li>Authenticate users with Facebook, Yahoo, Google, MSN, Twitter, MySpace and LinkedIn (more adding, soon!)<br>
</li><li>Access profile of logged in user (Email, First name, Last name, Profile pic, DOB and more)<sup>*</sup><br>
</li><li>Get access to AccessToken of logged in User<br>
</li><li>Consume REST API feeds of supported providers<br>
</li><li>Import friend contacts of logged in user (Email, Profile URL and Name)<sup>*</sup><br>
</li><li>Easily extend this library to add any provider supporting OAuth1.0a, Oauth1.0+OpenID Hybrid or OAuth2.0 protocols<br>
</li><li>Enable authentication on any existing quickly just be adding few DLLs and making some web.config changes<br>
</li><li>Easily design UI for login screen or let SocialAuth.NET do it automatically for you<br></li></ul>

<a href='Features_of_Version2.md'>Checkout</a> more features!<br>
<a href='http://socialauth-net.googlecode.com/svn/wiki/images/new.png'>http://code.google.com/p/socialauth-net/</a>
<sup>*</sup>depends on level of information exposed by selected provider<br>
<br>
<b>Be assured of all the support & help required in using SocialAuth.NET!</b><br>
<i>Please <a href='http://code.google.com/p/socialauth-net/issues/entry'>report</a> any issue and we promise to help you out!</i>

<a href='http://socialauth.in/socialauthdemo-net/Default.aspx'>Click here for a live demo</a> of how SocialAuth.NET works.<br>
<br>
<h1>How does it work?</h1>
Once SocialAuth.NET is integrated into your application, following is the authentication process:<br>
<ol><li>User requests for some restricted page<br>
</li><li>User is given an option to choose login via different popular service providers like Yahoo etc.<br>
</li><li>User selects a provider he is already registered with.<br>
</li><li>User is redirected to provider’s login site where they enter their credentials.<br>
</li><li>Upon successful login, provider takes user’s permission to share his basic data with your site<br>
</li><li>Once user accepts it, he returns back to your site to the same page which he initially requested for.<br>
This time user will be able to see page as he is now authenticated.</li></ol>

<img src='http://socialauth-net.googlecode.com/svn/wiki/images/Process_Flow.png' />


<h2>Who benefits from using SocialAuth.NET?</h2>
Using SocialAuth.NET benefits all of the following:<br>
<br>
<b>End users</b> – Instead of creating new account on website, users can use their existing accounts from popular providers like Facebook etc. making it easier for them to access restricted content.<br>
<br>
<b>Application Developers</b> – Application developers can add support for authentication through external providers with very simple integration procedure and be free from API intricacies of different providers. By calling standard methods, developers can also retrieve user profile and friends/contacts.<br>
<br>
<b>Site Owners</b> - With no registration requirement, users can quickly login to the site and use its features. Further, users trust big providers (like Facebook etc.) making them comfortable in login process. This fast & trusted login process definitely attracts potential users and increase site popularity.<br>
<br>
<h2>What features does SocialAuth.NET provide?</h2>
SocialAuth.NET provides following functional features:<br>
<ol><li>User authentication<br>
</li><li>Ability to access user’s profile (depends on provider selected for login)<br>
</li><li>Ability to access user’s contacts/friends (depends on provider selected for login)<br>
</li><li>Option to use SocialAuth.NET with WIF Security Token Service</li></ol>

<h2>Getting Started - How do I use SocialAuth.NET?</h2>
<ol><li><a href='http://opensource.brickred.com/socialauthdemo/'>Click here for a live demo</a> of how SocialAuth.NET works.<br>
</li><li>Liked the Demo? <a href='Quickstart_Guide.md'>Try SocialAuth.NET in your local environment</a>
</li><li>Convinced with SocialAuth.NET power? <a href='Integration_Guide.md'>Start developing</a> your own application<br>
</li><li>Already working with SocialAuth.NET ? <a href='writting_new_provider.md'>Add a new provider</a>!<br>
</li><li>Looking for a MVC - SocialAuth.NET quickstart? Click <a href='SocialAuthNet_With_ASPnetMVC.md'>here</a>
</li><li>Willing to explore options for SocialAuth.NET as STS? <a href='SocialAuth_STS_Overview.md'>Click here</a>!<br>
</li><li>We're also working on compilation of <a href='http://code.google.com/p/socialauth-net/wiki/FAQs'>FAQs</a>.<br>
</li><li>YouTube: <a href='http://www.youtube.com/watch?v=FOaUAKaSg0k'>How to try SocialAuth.NET Demo application on local machine</a></li></ol>

<h2>Acknowledgements</h2>
Development of SocialAuth.NET wouldn’t have been that easy as it turn out to be on using some of the external open source libraries:<br>
<ol><li><a href='http://oauth.googlecode.com/svn/code/csharp/OAuthBase.cs'>OAuth C# script</a>
</li><li><a href='http://json.codeplex.com/'>JSON.NET</a>
</li><li><a href='http://logging.apache.org/log4net/'>Log4Net</a>
Sincere thanks to authors of aforesaid libraries from SocialAuth.NET.<br />
Another great thanks to <b><a href='http://www.jetbrains.com'>JetBrains</a></b> for tooling us with addictive <img src='http://socialauth-net.googlecode.com/svn/wiki/images/resharper.png' /> and smart <img src='http://socialauth-net.googlecode.com/svn/wiki/images/dotcover.png' /> to help us code better & faster.</br>
<i>Haven't tried them yet? Try the awesome tools now!</i><br />
ReSharper - <a href='http://www.jetbrains.com/resharper/'>http://www.jetbrains.com/resharper/</a> </br>
DotCover - <a href='http://www.jetbrains.com/dotcover/'>http://www.jetbrains.com/dotcover/</a></br>
<h2>References</h2>
If you need help in registering your application with supported providers, please refer follwing links to get the API Keys:<br>
<ul><li>Google OAuth1.0 (old) <a href='http://code.google.com/p/socialauth/wiki/Google'>(show screenshot)</a> - <a href='http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html'>http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html</a>
</li><li>Google - <a href='https://code.google.com/apis/console'>https://code.google.com/apis/console</a>
</li><li>Yahoo <a href='http://code.google.com/p/socialauth/wiki/Yahoo'>(show screenshot)</a> - <a href='https://developer.apps.yahoo.com/dashboard/createKey.html'>https://developer.apps.yahoo.com/dashboard/createKey.html</a>
</li><li>Facebook  - <a href='http://www.facebook.com/developers/apps.php'>http://www.facebook.com/developers/apps.php</a>
</li><li>MSN LiveConnect - <a href='https://account.live.com/developers/applications'>https://account.live.com/developers/applications</a>
</li><li>Twitter - <a href='http://twitter.com/apps'>http://twitter.com/apps</a>
</li><li>MySpace - <a href='http://code.google.com/p/socialauth/wiki/MySpace'>(show screenshot)</a> - <a href='http://developer.myspace.com/Apps.mvc'>http://developer.myspace.com/Apps.mvc</a>
</li><li>LinkedIn - <a href='http://code.google.com/p/socialauth/wiki/Linkedin'>(show screenshot)</a> - <a href='https://www.linkedin.com/secure/developer'>https://www.linkedin.com/secure/developer</a></li></ul></li></ol>

<h2>Share it with your friends!!!</h2>
<wiki:gadget url="http://hosting.gmodules.com/ig/gadgets/file/113501407083818715381/Opti365ShareWebpageHelper.xml" border="0"/>