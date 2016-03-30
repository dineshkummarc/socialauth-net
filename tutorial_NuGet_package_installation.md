# Tutorial - Installing the SocialAuth NuGet Package #

This tutorial explains how you can quickly integrate SocialAuth.NET with you Website.

## Steps for GUI Based Installation ##
1.	Open your project to which SocialAuth.NET needs to be added in Visual Studio.

2.	Right-click your project in the solution explorer and select Manage NuGet packages from the context-menu. This opens the Manage NuGet Packages dialog. Type **SocialAuth.NET** in the search box on the top left, as shown in the following figure, and hit **Enter**:

![http://socialauth-net.googlecode.com/svn/wiki/images/NuGet_installationTutorial_screen1.png](http://socialauth-net.googlecode.com/svn/wiki/images/NuGet_installationTutorial_screen1.png)

3.	Select the version of SocialAuth that you want to install from the list displayed and click the **Install** button. Accept the licence agreement.

4.	The **Installing** dialog will be displayed, as shown in the following figure:


![http://socialauth-net.googlecode.com/svn/wiki/images/NuGet_installationTutorial_screen2.png](http://socialauth-net.googlecode.com/svn/wiki/images/NuGet_installationTutorial_screen2.png)

5.	Click the **Close** button in the **Manage NuGet Packages** dialog. SocialAuth.NET assemblies and configuration settings would then be available in your project. The updated project structure would now contain the following SocialAuth dlls:

![http://socialauth-net.googlecode.com/svn/wiki/images/NuGet_installationTutorial_screen3.png](http://socialauth-net.googlecode.com/svn/wiki/images/NuGet_installationTutorial_screen3.png)

Once the SocialAuth NuGet package has been installed, the site settings can be modified in order to integrate SocialAuth.NET with your site as follows:

## STEP – 1: Pre-requisite configuration steps ##



  1. Public domain - You will need a public domain for testing. You should have a public domain because most of the providers require a public domain to be specified when you register an application with them.<br><i>Note:</i> If you do not have a public domain jump to STEP-2 and after completing remaining steps refer to our guide - <a href='http://code.google.com/p/socialauth-net/wiki/Quickstart_Guide#Local_environment_setup'>How to try !SocialAuth.NET in local environment</a>.<br>
<ol><li>Get the API Keys: You can get the API keys from the following URLs.<br>
</li></ol><ul><li>Google <a href='http://code.google.com/p/socialauth/wiki/Google'>(show screenshot)</a> - <a href='http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html'>http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html</a>
</li><li>Yahoo <a href='http://code.google.com/p/socialauth/wiki/Yahoo'>(show screenshot)</a> - <a href='https://developer.apps.yahoo.com/dashboard/createKey.html'>https://developer.apps.yahoo.com/dashboard/createKey.html</a>
</li><li>Twitter - <a href='http://twitter.com/apps'>http://twitter.com/apps</a>
</li><li>Facebook - <a href='http://www.facebook.com/developers/apps.php'>http://www.facebook.com/developers/apps.php</a>
</li><li>Hotmail <a href='http://code.google.com/p/socialauth/wiki/Hotmail'>(show screenshot)</a> - <a href='http://msdn.microsoft.com/en-us/library/cc287659.aspx'>http://msdn.microsoft.com/en-us/library/cc287659.aspx</a>
</li><li><a href='http://code.google.com/p/socialauth/wiki/FourSquare'>FourSquare</a> - <a href='http://code.google.com/p/socialauth/wiki/FourSquare'>(show screenshot)</a> - <a href='https://foursquare.com/oauth/'>https://foursquare.com/oauth/</a>
</li><li><a href='http://code.google.com/p/socialauth/wiki/MySpace'>MySpace</a> - <a href='http://code.google.com/p/socialauth/wiki/MySpace'>(show screenshot)</a> - <a href='http://developer.myspace.com/Apps.mvc'>http://developer.myspace.com/Apps.mvc</a>
</li><li>Linkedin - <a href='http://code.google.com/p/socialauth/wiki/Linkedin'>(show screenshot)</a> - <a href='https://www.linkedin.com/secure/developer'>https://www.linkedin.com/secure/developer</a></li></ul>

You can now develop the application using keys and secrets obtained above and deploy the application on your public domain to test.<br>
<br>
<h2>STEP – 2: Update Web.Config file</h2>

Next step is to update with Web.Config with SocialAuth.NET configuration. For a detailed understanding, please refer this document.<br>
<br>
<h3>Mandatory Web.Config changes:</h3>
1.    Update API keys in the Provider section of SocialAuthConfiguration in the web.config file:<br>
<br>
<pre><code><br>
&lt;?xml version="1.0"?&gt;<br>
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
2.    If you are not using IIS7, remove handlers and modules under System.Webservers that were added by the NuGet package:<br>
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

<h3>Option – 1:  Changes for using SocialAuth.NET standard authentication engine and GUI</h3>

Update following element in SocialAuthConfiguration section created by the NuGet package:<br>
<br>
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



Update following element in SocialAuthConfiguration section created by the NuGet package:<br>
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
<h3>Option – 3:  Changes for using SocialAuth.NET standard authentication engine with Forms Authentication and custom GUI</h3>

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
<h3>Changes for using SocialAuth.NET API with MVC</h3>
In order to use SocialAuth.NET with MVC, the following changes are required apart from the ones mentioned above:<br>
<br>
Add following to Global.asax.cs in RegisterRoutes function:<br>
<pre><code>routes.IgnoreRoute("{*allaspx}", new { allaspx = @".*\.sauth(/.*)?" });<br>
</code></pre>
<h3>References:</h3>
<ul><li><a href='API_Reference.md'>API Reference</a>
</li><li><a href='WebConfig_Reference.md'>Web.Config Sections Anatomy</a>