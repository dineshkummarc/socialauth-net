# Create a new web application #
**Step-1:** Create a new web application in Visual Studio.

![http://socialauth-net.googlecode.com/svn/wiki/images/New_Project.png](http://socialauth-net.googlecode.com/svn/wiki/images/New_Project.png)

**Step-2:** Add a new aspx page "Default.aspx", put some text content and browse it.

**You should be able to view this page as there is no authentication at this point of time.**

# Configure SocialAuth.NET #

**Step-1:** http://code.google.com/p/socialauth-net/downloads/Download and extract Zip File of your preferred version (version 2.3 is recommended) and then add following DLLs from Lib folder as a reference to your project.
  * SocialAuth-net.DLL
  * Newtonsoft.Json.dll
  * Microsoft.IdentityModel.dll (Not required if you already have WIF SDK installed)
  * Log4net.dll (This is an optional dll. Refer it only if you want to enable Log4Net logging framework)

**Step-2:** Replace your web.config with the one provided in Zip inside Demo Folder.

**Compile your project.**

You are almost through!!

Now you need to register your application with providers you want to authenticate your site with. However, to quickly try SocialAuth.NET without registering application, proceed to following section.

# Local environment setup #
If you haven’t registered your application with any of the authentication provider, you may still try this application with our account. For that:<br><br>
1.	Open C:\Windows\System32\Drivers\etc\hosts file (in notepad) and add following entry:<br>
127.0.0.1	opensource.brickred.com<br>
(There is a Tab in between)<br>
<br>
<img src='http://socialauth-net.googlecode.com/svn/wiki/images/host_entry.png' />

2.	Open IIS (Run->inetmgr) and create a new application "Demo" under "Default Web Site" <i>(Right click Default Web Site and click Add Application)</i>. Set physical path to "C:\MyFolder\Demo" I.e. path to your demo project.<br>
<br>
<img src='http://socialauth-net.googlecode.com/svn/wiki/images/IIS.png' />

<img src='http://socialauth-net.googlecode.com/svn/wiki/images/demo_project.png' />

3.	Browse your Default.aspx again with URL as <a href='http://opensource.brickred.com/Demo/Default.aspx'>http://opensource.brickred.com/Demo/Default.aspx</a> in browser.<br>
<blockquote>For running this application from Visual Studio, Set the start options -> "custom site URL" property to <a href='http://opensource.brickred.com/Demo/Default.aspx'>http://opensource.brickred.com/Demo/Default.aspx</a></blockquote>

<b>Your site is now authentication enabled through so many popular providers. Hurray!</b>

Now when you will browse Default.aspx page, you should see a screen similar to following:<br>
<img src='http://socialauth-net.googlecode.com/svn/wiki/images/standard_login.png' />
Once you click on any provider, you will be redirected to that provider's login page. Enter credentials and you would be redirected back to Default.aspx authenticated.<br>
<br>
<h1>How it worked?</h1>
<ol><li>We modified Web.Config to add HttpHandler & HttpModule wired with SocialAuth.NET.<br>
</li><li>When user requested for page, HttpModule triggered to check if user is authenticated. Since user was not authenticated so far, request was passed to HttpHandler which displayed supported providers.<br>
</li><li>Once user selected a provider, SocialAuth.NET initialized an instance of wrapper for that provider and redirect user to provider for login.<br>
</li><li>On successful login, when request comes again to SocialAuth.NET, it allows user to view requested web page.