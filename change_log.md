# SocialAuth.NET Updates! #

  * **Version2.4 Minor Released on 28/July/2013**
    * Demo to post Tweet on Twitter updated to 1.1<br>
<ul><li>Google OAuth 2.0 support added<br>
</li><li>Control over base URL improved<br>
</li><li>Authentication Strategy updated to support POST versions (Facebook supports GET based OAuth strategy while some providers like Google and LinkedIn support POST based strategy)<br>
</li><li>An issue with unable to Tweet with space in between resolved<br>
</li><li><b>Very Important Note: If you've upgraded from a previous version and have been using Google Wrapper, you'd need to change the name of wrapper from GoogleWrapper to GoogleHybridWrapper in your config as we've upgraded Google Wrapper to use their OAuth 2.0 flow</b>. However, previous OAuth 1.0 Hybrid is still there. <br> Similarly, LinkedIn will automatically switch to OAuth2.0 (It doesn't need any explicit change). However, if you still want to use LinkedIn OAuth 1.0, change your LinkedIn wrapper name to LinkedIn1Wrapper in config.</li></ul></li></ul>


<ul><li><b>Version2.3.1a Hot Fix on 26/Jun/2013</b>
<ul><li>Twitter API upgraded to use their new 1.1 endpoint</li></ul></li></ul>

<ul><li><b>Version2.3.1 Patch Release on 18/Jan/2013</b>
<ul><li>Following bugs at <a href='http://code.google.com/p/socialauth-net/issues/list'>http://code.google.com/p/socialauth-net/issues/list</a> are resolved in this release:<br>
<ol><li>#91: Auto-Login not working for Twitter<br>
</li><li>157,85: Ability to use SocialAuth.NET by without altering UserIdentity on authentication (refer enhancement 1 below)<br>
</li><li>124: Ability to logout with specific provider (without application logout) when user is logged in with multiple providers (refer enhancement 2 below)<br>
</li><li>100: if a user is already logged in with a specific provider useing socialauth.net, attempt to login with that again would return without doing anything. Earlier user used to get redirected to default page.<br>
</li></ol></li><li>Enhancements:<br>
<ol><li>AllowModificationToUserIdentitify: By default SocialAuth.NET is a full-fledged authentication mechanism and when a user logs in with any provider, it marks UserIdentity as authenticated. If incase you want to keep application authentication separate and don't want SocialAuth.NET to alter UserIdentity in any way, you can add AllowModificationToUserIdentitify attribute to Authentication Tag in config as: <br>
<br>
<Authentication Enabled="true" LoginUrl="Default.aspx" DefaultUrl="Default.aspx" AllowModificationToUserIdentity="false" /><br>
<br>
 Note: a. By default it is true and SocialAuth.NET would continue to alter UserIdentity as it always does. b. You shouldn't make use of UserIdentity object to get SocialAuth data if you set above property.<br>
</li><li>New parameter has been added to Logout to allow user specify a particular provider they want to logout with. If that's the only provider, normal application level logout will take place and user will be redirected to login. However, if user is logged in with multiple provider and a specific provider is passed as parameter, user would continue to remain authenticated and will just get disconnected from specified provider.</li></ol></li></ul></li></ul>



<ul><li><b>Version2.3 Minor Released on 15/Jun/2012</b>
<ul><li>Now compatible with  sql and stateserver session stores<br>
</li><li>Login argument in Login() will now work in windows FormsAuthentication mode as well<br>
</li><li>NuGet package added<br>
</li><li>Bugs resolved<br>
</li></ul></li></ul><blockquote>- If you login using your custom/forms authentication mechanism, SocialAuth.NET wouldn't override Identity now<br></blockquote>

<ul><li><b>Version2.2 Minor Released on 27/Dec/2011</b>
<ul><li>Compatible with ASP.NET MVC (demo available in downloads)<br>
</li><li>Ability to specify Error URL while calling Login() <br>
</li><li>Option to exclude pages/folders from authentication when using integrated security<br></li></ul></li></ul>


<ul><li><b>Version2.1 Minor Released on 14/Nov/2011</b>
<ul><li>Persist tokens and restore them later (Demo project updated)<br>
</li><li>Custom Feed Execution enhanced for POST requests<br>
</li><li>Demo project updated to get and post Tweets on Twitter<br>
</li><li>Several issues fixed</li></ul></li></ul>


<ul><li><b>Version2.0 Major Released on 20/Sep/2011</b>
</li></ul><blockquote>- <a href='Features_of_Version2.md'>Checkout All Features of 2.x</a>