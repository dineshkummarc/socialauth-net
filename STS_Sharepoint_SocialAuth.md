Now you can use SocialAuth.NET as a Security Token Service (STS). STS is a form of web endpoint which facilitates delegation of authentication requirements to a separate centralized setup. Extraction of security framework from application and delegating security requirements to a centralized STS gives following benefits:

  * Centralized authentication endpoint for multiple applications
  * Communication being done using standard web protocols, STS can be utilized by applications of varied platforms
  * Easy maintenance with  single-point user management

To summarize, you can think of it as your hosted version of Microsoft Azure Access Control Service.

SocialAuth.NET along with default Windows WIF SDK leverages STS usability scenarios. You can implement ideas such as:

  * Login into SharePoint using Facebook
  * Centralize Login mechanism and provide ability to login through AD or OAuth provider

For detailed overview and tutorials, visit [STS section](SocialAuth_STS_Overview.md).