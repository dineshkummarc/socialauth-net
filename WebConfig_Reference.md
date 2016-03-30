# Introduction #

This page highlights various elements and tags of Web.Config that impact working of SocialAuth.NET

## Registering Providers ##
```
<?xml version="1.0"?>
<configuration>
  <configSections>
    <section name="SocialAuthConfiguration" type="Brickred.SocialAuth.NET.Core.SocialAuthConfiguration, SocialAuth-net, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null" allowDefinition="Everywhere" allowLocation="true" />
```
> Must specify the above tag to help SocialAuth.NET understand config file
```
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler" />
```
> This is an optional Log4Net configuration if you need logging.
```
  </configSections>
  <!-- Social Auth Configuration-->
  <SocialAuthConfiguration>
    <Providers>
      <add ScopeLevel="CUSTOM" WrapperName="GoogleWrapper"    ConsumerKey="ConsumerKey" ConsumerSecret="ConsumerSecret" AdditionalScopes=""  />
```
> Add Providers you want to use in your application. Though SocialAuth.NET as of now supports 7 providers, but only those providers will get activated which are specified here.
If you use SocialAuth.NET user control. it displays only those provider's icon listed here and in same sequence. Various attributes are:
  * **ScopeLevel** - This can be set to "DEFAULT" or "CUSTOM". Use "Custom", when you **do not wish** SocialAuth to add any scope automatically. Use "Default" when you wish SocialAuth to automatically add all scopes that are required for functioning of default features â€“ Contact & Profile. Irrespective of any ScopeLevel, you can also provide additional scopes in optional _AdditionalScopes_ attribute. For example, if you do not want any default scope for Google but want to access Calendar, your tag will look like: If ScopeLevel is not defined, SocialAuth.NET with work with default scopes with DEFAULT behavior.
  * **ConsumerKey** - ConsumerKey or ApplicationID provided from provider
  * **ConsumerSecret** - Secret provided from provider
  * **WrapperName** - `<ProviderName>` + "Wrapper". E.g., - FacebookWrapper, LinkedInWrapper etc.
  * **AdditionalScopes** - If you need to execute feed or use any REST API other than contacts or profiles, you can specify additional scopes here.
```
<Allow Files="abc.aspx|pqr.aspx" Folders="someFolder" />
```
Allow element is new in 2.2. By Default, when using SocialAuth integrated security, only one page can open until user logs in - Page specified in LoginURL. However, Allow element allows to specify pages and folders that should be skipped for security and open even when user is not logged in. Considering aforesaid example, following will be exempt from security check
  * "abc.aspx" and "pqr.aspx" irrespective of their folder location
  * Anything which is child of !SiteURL/!someFolder

## Images Folder Path for Demo User Control ##
```
	</Providers>
    <IconFolder Path="~/images/SocialAuthIcons/" />
```
> If you are using SocialAuth.NET user control, then this tag is required for picking up provider's icons.
```
    <Authentication Enabled="true" LoginUrl="Default.aspx" DefaultUrl="Default.aspx"  />
```
  * If Enabled = True, causes SocialAuth.NET framework to manage authentication of your site similar to FormsAuthentication. If you plan to use Forms Authentication or Custom Authentication, this should be set to false. In a reverse situation, when Enabled = True, .NET Authentication Mode  should be None.
  * LoginUrl: Url where user will redirected if not logged in. If Left blank, automatic UI is renderred.
  * DefaultUrl: Url where user will be redirected after login.

## Controlling URL ##

```
	<BaseURL Protocol="http"  Port="9090" Domain="opensource.brickred.com" Path="/Demo/Default.aspx"/>
```
> This tag allows to specify a domain that is used to serve all requests. If this tag is not defined, all URLs are relative to application site/virtual directory in IIS.

**Domain :** If specified, this domain is used otherwise IIS relative URL Host<br />
**Protocol :** If not specified, "http" is used.<br />
**Port :** If not specified, 80 is used for http and 443 for https<br />
**Path :** This gets appended to host if specified.

## Authentication Mode ##
If you are using Forms Based Authentication, that above tags should be added. It tells .NET to allow SocialAuth extension to work. If you are not using FormsBased authentication,
this tag can be skipped.
```
  <system.web>
    <sessionState mode="InProc" timeout="60" />
 
   <!--<authorization>
            <deny users="?" />
        </authorization>-->
    <!--<authentication mode="Forms">
	<forms loginUrl="ManualLogin.aspx" defaultUrl="welcome.aspx"/>
    </authentication>-->
    <!--<authentication mode="None"/>-->
```
Above set of tags is also optional if you are using Forms based Authentication. If not set Authentication mode = none.

```
  <compilation debug="true" targetFramework="4.0">
      <assemblies>
        <add assembly="Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
      </assemblies>
    </compilation>
    <httpHandlers>
      <add verb="*" path="*.sauth" type="Brickred.SocialAuth.NET.Core.CallbackHandler" />
    </httpHandlers>
    <httpModules>
      <add name="SocialAuthAuthentication" type="Brickred.SocialAuth.NET.Core.SocialAuthHttpModule" />
    </httpModules>
  </system.web>

  <!--IIS7 Support-->
  <system.webServer>
    <validation validateIntegratedModeConfiguration="false" />
    <handlers>
      <add name="socialAuth.NET" verb="*" path="*.sauth" type="Brickred.SocialAuth.NET.Core.CallbackHandler" />
    </handlers>
    <modules>
      <add name="SocialAuthAuthentication" type="Brickred.SocialAuth.NET.Core.SocialAuthHttpModule" />
    </modules>
  </system.webServer>
</configuration>
```
Add specified handlers and modules. If you are using IIS7 in classic mode, [refer to this thread](http://code.google.com/p/socialauth-net/issues/detail?id=11&can=1).

## Logging ##

```
	</SocialAuthConfiguration>

  <!--<log4net>
    <appender name="FileAppender" type="log4net.Appender.FileAppender">
      <file value="c:\log.txt" />
      <appendToFile value="true" />
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date &#9;[%property{SessionID}]&#9;%-5level&#9;%logger&#9;%message%newline" />
      </layout>
    </appender>
    -->
  <!-- Set root logger level to DEBUG and its only appender to A1 -->
  <!--
    <root>
      <level value="ALL" />
      <appender-ref ref="FileAppender" />
    </root>
  </log4net>-->
```

> If you wish to use Log4Net, use tags specified by it. To differentiate various tranactions, "!SessionID" is automatically set by SocialAuth.NET in Log4Net !threadcontext
```
  <!--<location path="SocialAuth">
        <system.web>
            <authorization>
                <allow users="*"/>
            </authorization>
        </system.web>
    </location>
```