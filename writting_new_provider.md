# STEP-1: Plan out Provider Integration #

Once you have finalized the provider you want to implement, you need to start gathering implementation details from provider perspective. This includes thinking about questions like:
  * Which protocol provider implements – OpenID/OAuth/Hybrid?
  * Where is the API documentation and implementation details located
  * Conceptualizing how this provider will be integrated with SocialAuth.NET framework

> As an example, if you are planning to implement MSN, following would all you need to find out:
  * MSN implements OAuth2.0 protocol and provides REST API Service.
  * Implementation and concepts of using MSN API are located at http://msdn.microsoft.com/en-us/library/ff752216.aspx
  * Since, OAuth protocol is a well supported aspect of SocialAuth.NET framework; we can get started with this integration!

# STEP-2: Register with Provider #
First pre-requisite to integration with any provider is to let the provider know your existence. I.e., you need to register your test/live application with the provider so as to obtain an Identity information.
All providers supporting OAuth protocols provide 2 pieces of information upon registration:
  1. ApplicationID (a.k.a. ConsumerKey)
  1. secret (a.k.a ConsumerSecret).
This is something you would need to find out from documentation of provider you wish to add. For MSN, we found Step-by-step illustration of registration with MSN at http://msdn.microsoft.com/en-us/library/ff751474.aspx
For MSN example, you can register application at Open https://manage.dev.live.com/AddApplication.aspx and register your application.

# STEP -3: Integrate Provider with SocialAuth.NET #

## 3.1 Update Provider Enumeration ##
Update the PROVIDER\_TYPE Enumeration with this new wrapper.<br>
<img src='http://socialauth-net.googlecode.com/svn/wiki/images/provider_enums.png' /><br>

<h2>3.2 Create a new wrapper class</h2>
Create a new simple class under WrapperFolder with convention <b>ProviderName</b> + <b>“Wrapper”</b>.cs.<br>
<img src='http://socialauth-net.googlecode.com/svn/wiki/images/wrapper_list.png' /><br>
So if you wish to add MSN, you would add MSNWrapper.cs to wrappers folder.<br>
<br>
<h2>3.3 Implement interface</h2>
Next step is to inherit your class from base class <b>Provider</b> and also implement <b>IProvider</b> interface.<br>
<pre><code> 	class MSNWrapper : Provider, IProvider<br>
</code></pre>

<br>
Use VisualStudio intellisense to quickly implement Base class.<br>
Right click on Provider and select "implement abstract class".<br>
<br>
This will add a bunch of properties and methods in your class.<br>
<br><br>
<h3>Configure Properties</h3>
Next step is to populate the configuration properties. SocialAuth.NET needs to know API Urls, for implementation of OAuth.<br>
<b>Authentication & configuration properties</b>
<table><thead><th>Property	</th><th>Description</th></thead><tbody>
<tr><td><i>PROVIDER_TYPE</i> <b>ProviderType</b></td><td>	This is most important. Specify the num value you created for new provider.</td></tr>
<tr><td><i>OAuthStrategyBase</i> <b>AuthenticationStrategy</b></td><td> Create and return an instance of OAuth protocol used by provider. Currently available classes are: OAuth1_0a, OAuth2_0 and OAuth1_0Hybrid</td></tr>
<tr><td><i>string</i> <b>RequestTokenEndpoint</b></td><td>	Specify provider's URL to obtain request token. For OAuth2.0 based providers, this value is not needed. </td></tr>
<tr><td><i>string</i> <b>UserLoginEndpoint</b></td><td> Provider's endpoint where user should be redirected for authentication </td></tr>
<tr><td><i>string</i> <b>AccessTokenEndpoint</b></td><td>	This is the URL to request for AccessToken once verification token (or code in OAuth2.0) has been retrieved after user has logged in </td></tr>
<tr><td><i><code>SIGNATURE_TYPE</code></i> <b>SignatureMethod</b></td><td>	This property identifies the encryption method to be used while sending request to API. There are 3 options: HMAC-SHA1, RSA-SHA1 and !Plaintext which means no encryption. MSN doesn’t requires encryption, hence we can use !plaintext.</td></tr>
<tr><td><i><code>TRANSPORT_METHOD</code></i> <b>TransportName</b></td><td>	This specifies whether request needs to be a POST or GET</td></tr></tbody></table>

<b>Data Retrieval properties</b>
<table><thead><th>Property	</th><th>Description</th></thead><tbody>
<tr><td><i>string</i> <b>ProfileEndpoint</b></td><td>	This specifies the URL to retrieve logged in user profiles. </td></tr>
<tr><td><i>string</i> <b>ContactsEndpoint</b></td><td>This specifies the API URL to get user contacts.  </td></tr>
<tr><td><i>string</i> <b>ProfilePictureEndpoint</b>	</td><td>If provider has a separate Endpoint for getting profile photo without including it in profile, specify it in this property</td></tr></tbody></table>



<h3>Implement Methods</h3>
Once properties are set, the next step is to implement methods.<br>
Methods reference:<br>
<br>
<table><thead><th>Method	</th><th>Description</th></thead><tbody>
<tr><td><i>BusinessObjects.UserProfile</i> <b>GetProfile()</b></td><td>	This method should contain code for retrieving user profile and populate profile object. In this method you would ideally use ExecuteFeed method to call rest api, parse response recevied (in text/Json or XML format) to ProfileObject and return. The best way is to retrieve token object using <br><b>Token token = SocialAuthUser.GetCurrentUser().GetConnection(this.ProviderType).GetConnectionToken()</b><br> and directly set the profie property of this object to keep it in session. To avoid multiple calls to profile, set the IsSet property to true. If set, profile is returned from session else everytime provider is called.</td></tr>
<tr><td><i>List<BusinessObjects.Contact></i> <b>GetContacts()</b></td><td>	This method should contain code for retrieving contacts (a.k.a friends, followers etc.). In this method you would ideally use ExecuteFeed method to call rest api, parse response recevied (in text/Json or XML format) to List of Contact and return.  Unlike profile, this data is not stored in session.</td></tr></tbody></table>

Notes:<br>
<ul><li>Instead of using ExecuteFeed, you may implement your own implementations for data retrieval methods.<br>
</li><li>You may just implement interface and write all required methods yourself (However, we have't tested this combination)<br>
</li><li>You can add more methods besides profile and contact by updating IProvider.</li></ul>

All methods can be easily implemented by referring 2 things:<br>
<ol><li>Specs of provider<br>
</li><li>Implementation of existing code to identify common code pattern and utility methods</li></ol>


<h1>STEP-4: Update Configuration</h1>
Open Web.config file and add new provider to existing list. If provider is XYZ:<br>
<pre><code> &lt;SocialAuthConfiguration&gt;<br>
    &lt;Providers&gt;<br>
      &lt;add WrapperName="MSNWrapper" ConsumerKey="YourKey" ConsumerSecret="YourSecret"/&gt;<br>
      &lt;add WrapperName="XYZWrapper" ConsumerKey="YourKey" ConsumerSecret="YourSecret"/&gt;<br>
    &lt;/Providers&gt;<br>
    &lt;IconFolder Path="~/images/SocialAuthIcons/"/&gt;<br>
</code></pre>
If you are using SocialAuth.NET user control, then ensure you have added .png icon with name of your provider enum in icon folder (path you have specified in SocialAuthConfiguration section of web.config)<br>
<br>
<h1>STEP-5: Test it</h1>
When application is run with above changes done, MSN would also appear along with other providers.