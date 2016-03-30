## Introduction ##

This page is a reference to using new SocialAuth.NET 2.0 API.
We have maintained backward compatibility with 1.x. However 2.0 provider many new methods and properties to incorporate 2.0 Features.
It is recommended that you go through new features before checking new API.

If you are using option - 1 of using SocialAuth.NET driven authentication and login screen. You would not need to use this API directly as everything is automatically wired up for you.
However, For other options you would need to have an understanding of various methods and properties. However, this can further be grouped into basic and advance sections.

## Easiest way to use SocialAuth.NET ##

Before I dig deeper into API, following code snippet will give you a fair idea on how you can use SocialAuth.NET API.

```
	 SocialAuthUser oUser = new SocialAuthUser(PROVIDER_TYPE.FACEBOOK); // Create an instance of SocialAuthUser for Facebook
	 oUser.Login(); //Call Login -> User will be redirected to provider for login
```

Once the user has logged In and he returns to default url:
```
	if(SocialAuthUser.IsLoggedIn()) // Safety check
	{
		SocialAuthUser oUser = SocialAuthUser.GetCurrentUser();
		UserProfile profile = oUser.GetProfile(); //Get Profile
		List<Contact> contacts = oUser.GetContacts(); //Get List of Contacts
		string accessToken = oUser.GetAccessToken(); //Get Access Token obtained from provider
		WebResponse wr = oUser.ExecuteFeed("http://RestAPIofProvider",TRANSPORT_METHOD.GET) // Execute a REST API at provider
		string responseOfFeed = new StreamReader(wr.GetResponseStream()).ReadToEnd();
		
	}
```

Isn't that easy to understand?? I know, you nodded Yes :)

## SocialAuth.NET Static Methods ##
Before you create an instance of SocialAuthUser to perform something, you can actually safeguard your code through new static methods introduces in 2.0.
Following is a summary of them.


```
PROVIDER_TYPE SocialAuthUser.CurrentProvider;

SocialAuthUser SocialAuthUser.GetCurrentUser();

bool SocialAuthUser.IsLoggedIn();

bool SocialAuthUser.IsConnectedWith(PROVIDER_TYPE);

List<PROVIDER_TYPE> PROVIDER_TYPE SocialAuthUser.GetConnectedProviders();
```

#### _PROVIDER\_TYPE_ CurrentProvider ####
If user is logged in into multiple providers, this property returns last PROVIDER\_TYPE, user connected with. If user is not connected with any provider it returns NOT\_SPECIFIED.
#### _SocialAuthUser_ GetCurrentUser() ####
As with 1.x, this method returns an instance of current user. If you call this method, and user is not connected with any provider, user will be redirected to login page.
#### _bool_ IsLoggedIn() ####
Returns true if user is logged in with any provider. It is recommended that you wrap your data retrieval code within this.
#### _bool_ IsConnectedWith(PROVIDER\_TYPE) ####
This methods helps you to check if user is logged in with a specific provider.
#### _`List<PROVIDER_TYPE>`_ GetConnectedProviders() ####
Returns a `List<PROVIDER_TYPE>` of all providers, user is logged in into.


## SocialAuth.NET Constructor ##

There are 2 options to instantiate SocialAuthUser object.
```
SocialAuthUser objUser = new SocialAuthUser(); // without any provider
SocialAuthUser objUser = new SocialAuthUser(PROVIDER_TYPE.FACEBOOK); //with one of supported provider
```

## Login ##
Once you have instantiated an object of objUser, you can invoke Login() to redirect user to provider.
Syntax
```
public void Login(PROVIDER_TYPE providerType = PROVIDER_TYPE.NOT_SPECIFIED, string returnUrl = "", Action callback = null, string !ErrorURL="");
```
All parameters in Login are optional. Hence, if you have 1.x code which accepted no parameter, it will continue to work.

| **Property** 				| **Description** |
|:-----------------|:----------------|
| **_PROVIDER\_TYPE_ provider** | Signifies the provider user wants to connect with. If not specified, provider specified in constructor will be used to login. You would also need this parameter to login user into multiple providers. Note: If this parameter is not specified and also no provider was provided in constructor, an error is thrown. |
| **_string_ returnUrl** 		| Specifies the URL, you want user to return after login. This URL applies only to specified provider's login.<br>Which means you can specify different return URLs for different providers.<br>URL specified can either be a full URL or if it is a page with in same application, it should be relative to root. For example, in a site www.abcd.com:&nbsp;eturnURL="<a href='http://www.google.com'>http://www.google.com</a>" will take user to google where as ReturmURL = "SomeFolder/welcome.aspx" will take user to www.abcd.com/SomeFolder/Welcome.aspx<br>If ReturnURL is not specified, user is redirected to DefaultURL specified in Web.Config.<br>Also note, that this option does not applies to FormsAuthentication. Irrespective of what is specified as value, default behavior of FormsAuthentication will override this value. <br>
<tr><td> <b><i>Action</i> Callback</b> 		</td><td> This is an exciting feature of 2.0. Now when calling Login() you can specify a method delegate which will automatically be called before user is authenticated and system is about to redirect user to return URL. You can use this method for linking purpose, overriding redirect behavior, executing some custom logic or for any other reason as per your need.<br>Note: Once Action Callback is specified, it applies for all subsequent login methods unless ResetCallback() is invoked </td></tr>
<tr><td> <b><i>ErrorURL</i></b> </td><td> This is a new argument in 2.2. User can specify any URL in this argument. If specified, on occurring of any error, user will be redirected to specified URL with error message and error type as argument to query string. This is particularly useful for creating widgets and is demonstrated in popup login demo </td></tr></tbody></table>

<h2>Data Feeds</h2>
Once user has logged in you can perform many other functions.<br>
<h4><i>UserProfile</i> GetProfile()</h4>
GetProfile() returns profile object from session. Profile is the only feed that is mashed up with Login() process and when user returns authenticated, his profile is stored into session. Every call to GetProfile() returns user profile from session.<br>
It has an overload of specifying PROVIDER_TYPE to retrieve profile of specific provider when user is connected with multiple providers.<br>
<h4><i><code>List&lt;Contact&gt;</code></i> GetContacts()</h4>
Returns list of contacts. Contacts has different meanings for different providers - Friends, Contacts etc.<br>
It has an overload of specifying PROVIDER_TYPE to retrieve contacts of specific provider when user is connected with multiple providers.<br>
<h4><i>string</i> GetAccessToken()</h4>
Returns AccessToken is obtained during login process for current or specified provider.<br>
<h4><i>IProvider</i> GetConnection()</h4>
This method returns a connection object that contains all properties of connected provider. All data feed methods can directly be executed on connection. For example<br>
<pre><code>	SocialAuthUser.GetCurrentUser().GetConnection(PROVIDER_TYPE.FACEBOOK).GetProfile()<br>
</code></pre>
However, it is less likely that you would use it as it is quite verbose and probably of use only for complex applications where you need consumer key, secret and other details.<br>
<h4><i>System.Net.WebResponse</i> ExecuteFeed()</h4>
This is yet another powerful feature of 2.0. SocialAuth.NET only supports Contacts and Profiles as of 2.0. If you want to execute REST API on provider, this is a feature for you.<br>
<pre><code>public WebResponse ExecuteFeed(string feedUrl, TRANSPORT_METHOD transportMethod, PROVIDER_TYPE providerType = PROVIDER_TYPE.NOT_SPECIFIED);<br>
</code></pre>

<table><thead><th> <b>Property</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> <b>string feedURL</b> </td><td> Endpoint of REST API you want to execute. </td></tr>
<tr><td> <b>TRANSPORT_METHOD method</b> </td><td> GET / POST         </td></tr>
<tr><td> <b>PROVIDER_TYPE provider</b> </td><td> Provider at which you want to execute feed. If left blank, feed execution is attempted on current provider (i.e., Last connected provider) </td></tr>
<tr><td> <b>byte<a href='.md'>.md</a> content</b> </td><td> Any information to be written to body of post</td></tr>
<tr><td> <b>Dictionary<string,string> headers</b> </td><td>Any headers to be added like contentType etc.</td></tr></tbody></table>

There are 2 overloaded versions. One with only first 3 aforesaid parameters which is ideal for GET requests. While second version (added in v2.1) is ideal for POST requests.<br>
<br>
<h2>LoadToken</h2>
Added new in v2.1, SocialAuthUser.GetCurrentUser().LoadToken(token) can be called with a populated Token parameter to automatically login user using details of token. This bypasses any !OAuth flow and sets user as logged in. If the token has expired or is invalid, any request to provider will fail. So, now once the user has logged in, you can serialize token to some repository and next time (with some mechanism to identify the same user), you can automatically get user logged in. Demo project contains more details on this.<br>
<br>
<h2>Logout</h2>
Logout() method can be use to reset user connections<br>
(Note: User may still remain connected to provider online but SocialAuth.NET will release all connections locally)<br>
<table><thead><th> <b>Property</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> <b>string loginUrl</b> </td><td> Any URL you want user to get redirected to after logout. If not specified, LoginUrl from config file is used. </td></tr>
<tr><td> <b>Action Callback</b> </td><td> Any Action you want to execute during SocialAuth.NET is clearing connections </td></tr>