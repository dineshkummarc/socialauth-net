# Introduction #

This page contains answers to frequently asked questions and ready-to-use sample code snippets (until they are incorporated in a release)

# Table of Contents #
<br>
<b>HOW TOs</b><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs?ts=1336201223&updated=FAQs#Posting_a_message_on_Facebook'>How to post on Facebook</a><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs#How_to_POST_on_Twitter'>How to post on Twitter</a><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs?ts=1341028638&updated=FAQs#Using_.NET_with_IIS7_in_classic_mode'>How to use SocialAuth.NET with IIS7 in classic mode</a><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs?ts=1350282395&updated=FAQs#How_to_post_a_Photo_on_Facebook_Wall'>How to Post a Photo on Facebook Wall</a>


<b>Troubleshooting</b><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs#Exception_Details:_Brickred._SocialAuth_.NET.Core.OAuthException'>Invalid Token Received error</a><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs?ts=1343982311&updated=FAQs#Error_on_retrieving_contacts_form_yahoo'>Error on retrieving contacts from Yahoo</a><br>
- <a href='http://code.google.com/p/socialauth-net/wiki/FAQs#Output_Cache_not_working'>Output Cache conflicting</a><br>
- <a href="http://code.google.com/p/socialauth-net/wiki/FAQs?ts=1346661918&updated=FAQs#STS_doesn't_shows_login_window">STS doesn't shows login window on next login attempt</a><br>
<br>
<b>Important Notes</b><br>
- Many hosting providers (for example GoDaddy) do not allow usage of  of custom HttpHandlers and HttpModules in shared hosting which is vital to functioning of SocialAuth.NET. It is recommended to ensure that your planned hosting provider supports them. There are many good hosts that allow execution of custom handlers/modules even in shared hosting.<br>
<br><br>
<br>
<br>
<hr><br>
<br>
<br>
<h3>Posting a message on Facebook</h3>
<pre><code>string message = HttpUtility.UrlEncode("Message text goes here");<br>
            string endpoint = "https://graph.facebook.com/me/feed?message=" + message + "&amp;access_token=" + SocialAuthUser.GetCurrentUser().GetConnection(PROVIDER_TYPE.FACEBOOK).GetConnectionToken().AccessToken ;<br>
<br>
            string body = String.Empty;<br>
            byte[] reqbytes = new ASCIIEncoding().GetBytes(body);<br>
            Dictionary&lt;string, string&gt; headers = new Dictionary&lt;string, string&gt;();<br>
            headers.Add("contentType", "application/x-www-form-urlencoded");<br>
            var response = SocialAuthUser.GetCurrentUser().ExecuteFeed(<br>
                    endpoint,<br>
                    TRANSPORT_METHOD.POST,<br>
                    PROVIDER_TYPE.FACEBOOK,<br>
                    reqbytes,<br>
                    headers<br>
                 );<br>
<br>
</code></pre>


You'd need to ensure that user is logged in to Facebook before executing above code. You can easily do this by wrapping this code within<br>
<pre><code>if (!SocialAuthUser.IsConnectedWith(PROVIDER_TYPE.FACEBOOK))<br>
{<br>
 ...code<br>
}<br>
</code></pre>

API Reference URL: <a href='http://developers.facebook.com/docs/reference/api/post/'>http://developers.facebook.com/docs/reference/api/post/</a>
<br>
<br>
<hr><br>
<br>
<br>
<br>
<br>
<hr><br>
<br>
<br>
<h3>How to POST on Twitter</h3>
<pre><code> string msg = HttpUtility.UrlEncode("Message text goes here");<br>
            string endpoint = "http://api.twitter.com/1/statuses/update.json?status=" + msg;<br>
<br>
            string body = String.Empty;<br>
            byte[] reqbytes = new ASCIIEncoding().GetBytes(body);<br>
            Dictionary&lt;string, string&gt; headers = new Dictionary&lt;string, string&gt;();<br>
            headers.Add("contentType", "application/x-www-form-urlencoded");<br>
            var response = SocialAuthUser.GetCurrentUser().ExecuteFeed(<br>
                    endpoint,<br>
                    TRANSPORT_METHOD.POST,<br>
                    PROVIDER_TYPE.TWITTER,<br>
                    reqbytes,<br>
                    headers<br>
                 );<br>
</code></pre>

<h3>How to post a Photo on Facebook Wall</h3>
<pre><code> //Some submit button<br>
        protected void Button1_Click(object sender, EventArgs e)<br>
        {<br>
            byte[] imgData;<br>
			//You have to get in your image bytes to imgData variable.<br>
			//Following code is an example of filling this variable with photo bytes uploaded through ASPFileUpload control<br>
            imgData = FileUpload1.FileBytes;<br>
<br>
<br>
            // Create Boundary<br>
            string boundary = "---------------------------" + DateTime.Now.Ticks.ToString("x");<br>
<br>
            // Create Path<br>
            string Path = @"https://graph.facebook.com/me/photos";<br>
<br>
            // Create HttpWebRequest<br>
            HttpWebRequest uploadRequest;<br>
            uploadRequest = (HttpWebRequest)HttpWebRequest.Create(Path);<br>
            uploadRequest.Method = "POST";<br>
            uploadRequest.ContentType = "multipart/form-data; boundary=" + boundary;<br>
            uploadRequest.KeepAlive = false;<br>
<br>
            // New String Builder<br>
            StringBuilder sb = new StringBuilder();<br>
<br>
            // Add Form Data<br>
            string formdataTemplate = "--{0}\r\nContent-Disposition: form-data; name=\"{1}\"\r\n\r\n{2}\r\n";<br>
<br>
            // Access Token<br>
            sb.AppendFormat(formdataTemplate, boundary, "access_token", SocialAuthUser.GetCurrentUser().GetAccessToken(PROVIDER_TYPE.FACEBOOK));<br>
<br>
            // Message<br>
            sb.AppendFormat(formdataTemplate, boundary, "message", "SocialAuthPhoto");<br>
<br>
            // Header<br>
            string headerTemplate = "--{0}\r\nContent-Disposition: form-data; name=\"{1}\"; filename=\"{2}\"\r\nContent-Type: {3}\r\n\r\n";<br>
            sb.AppendFormat(headerTemplate, boundary, "source", FileUpload1.FileName, @"application/octet-stream");<br>
<br>
            // File<br>
            string formString = sb.ToString();<br>
            byte[] formBytes = Encoding.UTF8.GetBytes(formString);<br>
            byte[] trailingBytes = Encoding.UTF8.GetBytes("\r\n--" + boundary + "--\r\n");<br>
            byte[] image = imgData;<br>
<br>
            // Memory Stream<br>
            MemoryStream imageMemoryStream = new MemoryStream();<br>
            imageMemoryStream.Write(image, 0, image.Length);<br>
<br>
            // Set Content Length<br>
            long imageLength = imageMemoryStream.Length;<br>
            long contentLength = formBytes.Length + imageLength + trailingBytes.Length;<br>
            uploadRequest.ContentLength = contentLength;<br>
<br>
            // Get Request Stream<br>
            uploadRequest.AllowWriteStreamBuffering = false;<br>
            Stream strm_out = uploadRequest.GetRequestStream();<br>
<br>
            // Write to Stream<br>
            strm_out.Write(formBytes, 0, formBytes.Length);<br>
            byte[] buffer = new Byte[checked((uint)Math.Min(4096, (int)imageLength))];<br>
            int bytesRead = 0;<br>
            int bytesTotal = 0;<br>
            imageMemoryStream.Seek(0, SeekOrigin.Begin);<br>
            while ((bytesRead = imageMemoryStream.Read(buffer, 0, buffer.Length)) != 0)<br>
            {<br>
                strm_out.Write(buffer, 0, bytesRead); bytesTotal += bytesRead;<br>
            }<br>
            strm_out.Write(trailingBytes, 0, trailingBytes.Length);<br>
<br>
            // Close Stream<br>
            strm_out.Close();<br>
<br>
            // Get Web Response<br>
            HttpWebResponse response = uploadRequest.GetResponse() as HttpWebResponse;<br>
<br>
            // Create Stream Reader<br>
            StreamReader reader = new StreamReader(response.GetResponseStream());<br>
            JObject responseJson = JObject.Parse(reader.ReadToEnd());<br>
            Response.Write(responseJson["id"].ToString());<br>
        }<br>
<br>
</code></pre>
<h3>Using SocialAuth.NET with IIS7 in classic mode</h3>

To use SocialAuth.NET on IIS7 running in classic mode, change !httphandler configuration  (under System.WebServers) as following:<br>
<br>
<pre><code>  &lt;system.webServer&gt;<br>
...<br>
    &lt;add name="socialAuth.NET" verb="*" path="*.sauth" modules="IsapiModule" type="Brickred.SocialAuth.NET.Core.CallbackHandlers"  scriptProcessor="C:\Windows\Microsoft.NET\Framework64\v4.0.30319\aspnet_isapi.dll" /&gt;<br>
...<br>
</code></pre>
note: scriptprocessor should correctly point to path where your aspnet_isapi.dll of relevant .NET framework resides.<br>
<br>
<br>
<br>
<hr><br>
<br>
<br>
<blockquote><h2><font color='red'>Exception Details: Brickred.SocialAuth.NET.Core.OAuthException: Invalid Request Token received.</font></h2>
This issue happens when Google doesn't returns oauth_request_token after user has successfully logged in. So far, we've found this to occur only when consumer Key passed doesn't matches (exactly) the registered key. Following are a few tips to resolve this:<br>
</blockquote><ul><li>If you're doing a copy paste of key from browser to config, ensure that there are no spaces copied at beginning/end of consumer key.<br>
</li><li>URL registered and matched should exactly match what's there in config. For example, if it is www.site.com the same should be in config (and likely in same case). Putting <a href='http://site.com'>http://site.com</a> OR <a href='http://www.site.com'>http://www.site.com</a> or site.com may result in such exception<br>
</li><li>Please register separate key for sub domains i.e. if you have registered www.somesite.com, it can not be used for www.sub.somesite.com. A separate key/secret should be generated for later.<br>
</li><li>Recommended usto use  <a href='http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html'>http://code.google.com/apis/accounts/docs/RegistrationForWebAppsAuto.html</a> for registering applications<br>
<i>Thanks to <b>Kelvin</b> and <b>Nitin</b> for cracking this</i>
<br />
Update: SocialAuth.net now supports Google OAuth 2.0. Refer <a href='https://code.google.com/p/socialauth-net/wiki/google_provider_invalid_token'>https://code.google.com/p/socialauth-net/wiki/google_provider_invalid_token</a> for details.<br>
<br>
<br>
<hr><br>
<br>
<br>
</li></ul><blockquote><h2><font color='red'>Error on retrieving contacts form yahoo</font></h2>
Unlike other providers which respect scope passed as argument while making a connection, Yahoo requires scopes to be predefined from Yahoo UI itself. If you are seeing error on retrieving contacts, please ensure you have permissions enabled (refer following screenshot)<br>
<a href='http://socialauth-net.googlecode.com/svn/wiki/images/yahoo.PNG'>http://socialauth-net.googlecode.com/svn/wiki/images/yahoo.PNG</a>
<br>
<br>
<hr><br>
<br>
<br>
<h2>Output Cache not working</h2>
If you're using Forms based authentication, and find issues with outpucache, remove SocialAuthHttpModule from config. This module is not required with forms based authentication.<br>
<br>
<br>
<hr><br>
<br>
<br>
<h2>STS doesn't shows login window</h2>
If you want STS to always display login popup, call the logout method before STS redirects user to your ASP.NET or Sharepoint site.<br>
<br>Reference: <a href='http://code.google.com/p/socialauth-net/issues/detail?id=104'>ISSUE-103</a>