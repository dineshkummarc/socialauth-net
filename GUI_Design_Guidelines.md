# Designing Login GUI #

Login GUI in SocialAuth.NET context is mere listing of supported providers. When user selects any provider (for example by clicking on it), SocialAuth.NET takes user to respective provider’s login screen.

Displaying providers to choose from can be a listing of text links or buttons or images or any other format you would wish to use.  For example:

![http://socialauth-net.googlecode.com/svn/wiki/images/gui_example.png](http://socialauth-net.googlecode.com/svn/wiki/images/gui_example.png)

SocialAuth.NET GUI generation is very easy and allows using any of the following:

  1. JavaScript and simple HTML controls
  1. ASP.NET server controls
  1. SocialAuth.NET Web user control

You might wish to wrap up your GUI in a user control or server control or have controls place directly on web page. Controls for different providers can be placed hardcoded in HTML or can be generated dynamically through available provider’s collection.

Following sections illustrate step-by-step guide to designing your login screen/control.

## Option – 1 Design GUI with simple HTML controls and JavaScript ##
This option illustrates placing simple HTML buttons and wiring events.
Add following JavaScript function to your common .JS file or inside script tag within your HTML:
```
function Login(providerName)
        {
            window.location.href='login.sauth?p=' + providerName
        }
  
```
Next part is displaying controls. You can use HTML as simple as following to design UI or make it as advance as your creative thoughts could be:
```
<input type="button" value="facebook" OnClick="Login('facebook')"/>
<input type="button" value="google" OnClick="Login('google')"/>
<input type="button" value="yahoo" OnClick="Login('yahoo')"/
```

This will display following:
![http://socialauth-net.googlecode.com/svn/wiki/images/gui_buttons.png](http://socialauth-net.googlecode.com/svn/wiki/images/gui_buttons.png)

And you are done!!

Note: Parameter passed to Login function should be exact name of provider as used in framework. Currently supported values are – “facebook”, “google” , “yahoo”and “msn”.


## Option – 2 Design GUI with ASP.NET controls ##

Instead of using HTML controls, you can also use .NET controls for generating UI and also dynamically on the basis of available providers. Following example illustrates displaying similar UI as option 1 but using ASP.NET controls.
Place your ASP.NET controls:
```
<asp:Button ID="btnFacebook" Text="Facebook" runat="server" OnClick="btnFacebook_Click" />    
<asp:Button ID="btnGoogle" Text="Google" runat="server" OnClick="btnGoogle_Click" />         
<asp:Button ID="btnYahoo" Text="Yahoo" runat="server" OnClick="btnYahoo_Click" />
```
Wire them up with following:
```
    protected void btnFacebook_Click(object sender, EventArgs e)
    {
        SocialAuthUser oUser = new SocialAuthUser(PROVIDER_TYPE.FACEBOOK);
        oUser.Login();
    }
 
    protected void btnGoogle_Click(object sender, EventArgs e)
    {
        SocialAuthUser oUser = new SocialAuthUser(PROVIDER_TYPE.GOOGLE);
        oUser.Login("Welcome.aspx");
    }
 
    protected void btnYahoo_Click(object sender, EventArgs e)
    {
        SocialAuthUser oUser = new SocialAuthUser(PROVIDER_TYPE.YAHOO);
        oUser.Login();
    }

```
## Dynamically generating controls ##
For any of the above options, controls can be generated dynamically also based on providers listed in Web.Config. Following code provides list of all providers:
**SocialAuth.ProviderFactory.Providers** which returns <pre>List<Providers></pre>

## Option – 3 Using Web User Control ##

SocialAuth.NET also provides ready to user web user control which you can drag drop in your ASPX pages. This control automatically scans available providers and displays controls accordingly. In order to use this User Control, you need to do 2 additional steps, besides adding control to you page
  1. Copy “SocialAuthIcons” folder in your web application directory. It contains icons for various providers used to generate user control. You can change these icons with any image but preserving name.
  1. Update SocialAuthConfiguration Section in Web.Config with IconsFolder tag
```
<SocialAuthConfiguration>
        <Providers>
	...
        </Providers>
        <IconFolder Path="~/images/SocialAuthIcons/" />
</SocialAuthConfiguration>
```
**_Iconfolder_**  Path attribute must point to “SocialAuthIcons” folder relative to your application root directory as illustrated in above example.
You can go through code of web user control to get a better understanding on how to generate controls for listing providers dynamically.
As a note, if you do not want to design your own GUI set the LoginUrl attribute to blank in Web.Config and SocialAuth.NET will automatically generate following screen for your users. This option is applicable only when you are using just SocialAuth.NET authentication mechanism without any other mechanism like FormsAuthentication.
![http://socialauth-net.googlecode.com/svn/wiki/images/standard_login.png](http://socialauth-net.googlecode.com/svn/wiki/images/standard_login.png)
```
<SocialAuthConfiguration>
 ...
        <Authentication Enabled="true" LoginUrl="" DefaultUrl="Welcome.aspx" />
</SocialAuthConfiguration>
```
You do not need to set IconFolder path in this case.