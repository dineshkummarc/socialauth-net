#summary Detailed guide for using SocialAuth.NET in your applications



# Three steps to integrate SocialAuth.NET #



Using SocialAuth.NET is a merely a three step activity



  1. Register your website with service providers  you want to use (SocialAuth.NET currently officially supports Google, Facebook, MSN , LinkedIn, Yahoo and MySpace)
  1. Add a reference to required DLLs in your web application
  1. Integrate SocialAuth.NET to your application through some web.config changes and optional GUI designing



…and you are actually through!!



_Note: Steps 1 and 2 are common for SocialAuth.NET integration, while steps 3 has different options to choose from._

# Integration options overview #

SocialAuth.NET provides 3+1 options to integrate SocialAuth.NET with ASP.NET application.

  1. **Use SocialAuth.NET standard authentication engine and GUI**<br>This is the simplest and coolest way to use SocialAuth.NET. If you have any website with currently no authentication, or starting up with a new site, using this option you do not need to write even a single piece of code for implementing authentication. SocialAuth.NET takes care of authentication mechanism and login screen.<br><br>
<ol><li><b>Use SocialAuth.NET standard authentication engine but custom designed GUI</b><br>This option allows you to design you own provider selection screen, call SocialAuth.NET API and let SocialAuth.NET to do rest of the task<br><br>
</li><li><b>Use SocialAuth.NET authentication with existing Forms based Authentication</b> <br>If you already have a running site with user authentication implemented via .net FormsAuthentication, but now want to allow users to login via service providers as well, this is the option for you. Modify your login screen to allow login through providers as well, add a few tags to Web.Config and you are through.<br><br>
</li><li><b>Use SocialAuth.NET API with custom authentication & screen</b> <br>This is not a standard option. However if you don’t wish to use either automatic SocialAuth.NET or Forms based authentication, you can use methods of SocialAuth.NET API and implement your own custom authentication framework. For example, bare basic authentication could be applying<br>
<pre><code>SocialAuthUser.IsLoggedIn() <br>
</code></pre>
on all pages you want to authenticate and if a user is not logged in, redirecting user to custom screen with some control that invokes Login() of SocialAuth.NET<br></li></ol>



<b>Login screen</b> above refers to any UI element displaying service provider options for user to choose from. So it could be a list of providers’ icon, simple asp.net button with provider name or any other GUI element you wish to use. This is so because, actual login credentials input screen will be rendered by service provider itself on their site.<br>
<br>
<br>
<br>
<b>Note:</b> Aforesaid integration options will be guiding factor for integration steps.<br>
<br>
<br>
<br>
<h1>Steps for integration</h1>

<h2>STEP – 1: Pre-requisite configuration steps</h2>



<ol><li>Public domain - You will need a public domain for testing. You should have a public domain because most of the providers require a public domain to be specified when you register an application with them.<br><i>Note:</i> If you do not have a public domain jump to STEP-2 and after completing remaining steps refer to our guide - <a href='http://code.google.com/p/socialauth-net/wiki/Quickstart_Guide#Local_environment_setup'>How to try !SocialAuth.NET in local environment</a>.<br>
</li><li>Get the API Keys: You can get the API keys from the following URLs.<br>
</li></ol><ul><li>Google <a href='http://code.google.com/p/socialauth/wiki/Google'>(show screenshot)</a> - <a href='http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html'>http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html</a>
</li><li>Yahoo <a href='http://code.google.com/p/socialauth/wiki/Yahoo'>(show screenshot)</a> - <a href='https://developer.apps.yahoo.com/dashboard/createKey.html'>https://developer.apps.yahoo.com/dashboard/createKey.html</a>
</li><li>Twitter - <a href='http://twitter.com/apps'>http://twitter.com/apps</a>
</li><li>Facebook - <a href='http://www.facebook.com/developers/apps.php'>http://www.facebook.com/developers/apps.php</a>
</li><li>Hotmail <a href='http://code.google.com/p/socialauth/wiki/Hotmail'>(show screenshot)</a> - <a href='http://msdn.microsoft.com/en-us/library/cc287659.aspx'>http://msdn.microsoft.com/en-us/library/cc287659.aspx</a>
</li><li><a href='http://code.google.com/p/socialauth/wiki/FourSquare'>FourSquare</a> - <a href='http://code.google.com/p/socialauth/wiki/FourSquare'>(show screenshot)</a> - <a href='https://foursquare.com/oauth/'>https://foursquare.com/oauth/</a>
</li><li><a href='http://code.google.com/p/socialauth/wiki/MySpace'>MySpace</a> - <a href='http://code.google.com/p/socialauth/wiki/MySpace'>(show screenshot)</a> - <a href='http://developer.myspace.com/Apps.mvc'>http://developer.myspace.com/Apps.mvc</a>
</li><li>Linkedin - <a href='http://code.google.com/p/socialauth/wiki/Linkedin'>(show screenshot)</a> - <a href='https://www.linkedin.com/secure/developer'>https://www.linkedin.com/secure/developer</a>
</li></ul><ol><li>You can now develop the application using keys and secrets obtained above and deploy the application on your public domain to test.</li></ol>



<h2>STEP – 2: Refer required DLLs to your web project</h2>



Assuming that you have already created a new web application (or opened an existing one), next step is to add few DLL references in your application. These DLLs are available inside Lib folder of SocialAuth-net-2.x.zip file. Following is the list of DLLs required:<br>
<br>
<ol><li>SocialAuth-net-2.x.DLL<br>
</li><li>Newtonsoft.Json.dll<br>
</li><li>Microsoft.IdentityModel.dll (Not required of you already have WIF SDK installed)<br>
</li><li>log4net.dll (This is an optional dll. Refer it if you want to enable Log4Net logging framework)</li></ol>



<h2>STEP – 3: Update Web.Config file</h2>

Next step is to update with Web.Config with SocialAuth.NET configuration and required httpModules and httpHandlers. For a detailed understanding, please refer this document.<br>
<br>
<h3>Mandatory Web.Config changes:</h3>

1.    Add SocialAuth SectionHandler and configuration blocks:<br>
<br>
<pre><code><br>
&lt;?xml version="1.0"?&gt;<br>
<br>
&lt;configuration&gt;<br>
<br>
    &lt;configSections&gt;...<br>
<br>
       &lt;section name="SocialAuthConfiguration" type="Brickred.SocialAuth.NET.Core.SocialAuthConfiguration, SocialAuth-net, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null" allowDefinition="Everywhere" allowLocation="true" /&gt;<br>
<br>
... &lt;/configSections&gt;<br>
<br>
<br>
<br>
&lt;!-- Social Auth Configuration--&gt;<br>
<br>
 &lt;SocialAuthConfiguration&gt;<br>
<br>
     &lt;Providers&gt;<br>
<br>
      &lt;add WrapperName="FacebookWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
      &lt;add WrapperName="GoogleWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
      &lt;add WrapperName="MSNWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
      &lt;add WrapperName="YahooWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
      &lt;add WrapperName="TwitterWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
      &lt;add WrapperName="LinkedInWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
      &lt;add WrapperName="MySpaceWrapper" ConsumerKey="Key" ConsumerSecret="Secret" /&gt;<br>
<br>
<br>
<br>
    &lt;/Providers&gt;<br>
<br>
 &lt;/SocialAuthConfiguration&gt;...<br>
<br>
&lt;/configuration&gt;<br>
<br>
</code></pre>



2.    Add httpHandlers and httpModules under System.Web<br>
<br>
<pre><code><br>
&lt;httpHandlers&gt;<br>
<br>
   &lt;add verb="*" path="*.sauth" type="Brickred.SocialAuth.NET.Core.CallbackHandler" /&gt;<br>
<br>
&lt;/httpHandlers&gt;<br>
<br>
&lt;httpModules&gt;<br>
<br>
     &lt;add name="SocialAuthAuthentication" <br>
<br>
type="Brickred.SocialAuth.NET.Core.SocialAuthHttpModule" /&gt;<br>
<br>
&lt;/httpModules&gt;<br>
<br>
</code></pre>

3.    If you are using IIS7, add handlers and modules under System.Webservers as well<br>
<br>
<pre><code><br>
&lt;system.webServer&gt;<br>
<br>
  &lt;validation validateIntegratedModeConfiguration="false" /&gt;<br>
<br>
  &lt;handlers&gt;<br>
<br>
     &lt;add name="SocialAuth.NET" verb="*" path="*.sauth" type="Brickred.SocialAuth.NET.Core.CallbackHandler" /&gt;<br>
<br>
  &lt;/handlers&gt;<br>
<br>
  &lt;modules&gt;<br>
<br>
&lt;add name="SocialAuthAuthentication" type="Brickred.SocialAuth.NET.Core.SocialAuthHttpModule" /&gt;<br>
<br>
&lt;/modules&gt;<br>
<br>
&lt;/system.webServer&gt;<br>
<br>
</code></pre>

4.    Configure your site to let only authenticated users access it<br>
<br>
<pre><code><br>
&lt;system.web&gt;...<br>
<br>
        &lt;authorization&gt;<br>
<br>
            &lt;deny users="?" /&gt;<br>
<br>
        &lt;/authorization&gt;<br>
<br>
</code></pre>

Aforesaid changes are mandatory irrespective of which integration option. Following changes are specific to integration option you choose. If you are switching from one option to another, revert back all previous changes to version with only mandatory changes done.<br>
<br>
<br>
<br>
<h3>Option – 1:  Changes for using SocialAuth.NET standard authentication engine and GUI</h3>

Add following element in SocialAuthConfiguration section created in mandatory change 1.<br>
<br>
<pre><code><br>
&lt;SocialAuthConfiguration&gt;<br>
<br>
   ...<br>
<br>
     &lt;Authentication Enabled="true" LoginUrl="" DefaultUrl="Welcome.aspx" /&gt;<br>
<br>
 &lt;/SocialAuthConfiguration&gt;<br>
<br>
</code></pre>

<b><i>Notes:</i></b>

<ul><li>Ensure that you have no other authentication enabled.</li></ul>

<pre><code><br>
&lt;authentication mode="None" /&gt;<br>
<br>
</code></pre>

<ul><li>Set <b><i>DefaultUrl</i></b> to page where user will be redirected to after login</li></ul>





<h3>Option – 2:  Changes for using SocialAuth.NET standard authentication engine and custom GUI development</h3>



Add following element in SocialAuthConfiguration section created in mandatory change 1.<br>
<br>
<pre><code><br>
&lt;SocialAuthConfiguration&gt;<br>
<br>
   ...<br>
<br>
    &lt;Authentication Enabled="true" LoginUrl="Login.aspx" DefaultUrl="Welcome.aspx" /&gt;<br>
<br>
 &lt;/SocialAuthConfiguration&gt;<br>
<br>
</code></pre>



<b><i>Notes:</i></b>

<ul><li>Ensure that you have no other authentication enabled.</li></ul>

<pre><code><br>
&lt;authentication mode="None" /&gt;<br>
<br>
</code></pre>

<ul><li>Set <b><i>DefaultUrl</i></b> to page user will be redirected to after login</li></ul>

<ul><li>Set <b><i>LoginUrl</i></b> to login page with your custom GUI.</li></ul>



<a href='GUI_Design_Guidelines.md'>Refer here on how to make GUI</a> for displaying list of providers for user to login.<br>
<br>
<br>
<br>
<h3>Option – 3:  Changes for using SocialAuth.NET standard authentication engine with Forms Authentication and custom GUI</h3>



<ol><li>Add following block under Configuration tag to allow SocialAuth internals to bypass authentication.<br></li></ol>

<pre><code><br>
&lt;location path="SocialAuth"&gt;<br>
<br>
    &lt;system.web&gt;<br>
<br>
      &lt;authorization&gt;<br>
<br>
        &lt;allow users="*"/&gt;<br>
<br>
      &lt;/authorization&gt;<br>
<br>
    &lt;/system.web&gt;<br>
<br>
  &lt;/location&gt;<br>
<br>
</code></pre>

<ol><li>Remove !Brickred.SocialAuth.NET.Core.SocialAuthHttpModule HttpModule tags from Web.Config (added in pre-requisite steps 3 and 4)</li></ol>

<b><i>Notes:</i></b>

<ul><li>Ensure that you have Forms authentication enabled.</li></ul>

<pre><code><br>
&lt;authentication mode="Forms" /&gt;<br>
<br>
</code></pre>

<ul><li>Set <b><i>DefaultUrl</i></b> to page user will be redirected to after login</li></ul>

<ul><li>Add GUI for SocialAuth.NET to your existing login screen.<a href='GUI_Design_Guidelines.md'>Refer here on how to make GUI</a> for displaying list of providers for user to login.</li></ul>

<h3>Option – 4:  Changes for using SocialAuth.NET API with custom  authentication and GUI</h3>
If .NET Authentication Mode is set to "None" and in SocialAuth section, Authentication tag's Enabled attribute is set to False, SocialAuth.NET assumes that option-4 is active. It does nothing automatically in this mode and expect user to call API methods as required.<br>
<br>
<br>
<h3>References:</h3>
<ul><li><a href='API_Reference.md'>API Reference</a>
</li><li><a href='WebConfig_Reference.md'>Web.Config Sections Anatomy</a>