# Introduction #

**Using SocialAuth.NET with ASP.NET MVC framework is very easy**. The following are basic steps outlining the same for a newly created ASP.NET MVC application:

  * Add reference to required DLLs
  1. SocialAuth-net.DLL
  1. Newtonsoft.Json.dll
  1. Microsoft.IdentityModel.dll (Not required if you already have WIF SDK installed)
  1. Log4net.dll (This is an optional dll. Refer it only if you want to enable Log4Net logging framework)

  * Update Web.Config with:
  1. SocialAuth configuration tags (Refer to integration guide/ web.config reference Wiki and even better - demo application's web.config for reference on tags)
  1. Registration of handlers and modules
  1. Update of keys and secrets (if you wish try with your keys)

  * Add following to Global.asax.cs in RegisterRoutes function
```
routes.IgnoreRoute("{*allaspx}", new { allaspx = @".*\.sauth(/.*)?" });
```
Note: Ensure aforesaid line is added as early when method body starts (specially it should be added before routes.MapRoute() is called)

Above 3 steps are sufficient to make SocialAuth.NET work with your ASP.NET MVC application in 2 modes (same as in ASP.NET webforms):
  1. SocialAuth.NET integrated security
  1. FormsAuthentication authentication

**Please raise your queries and suggestions in issues list so that we can improve this SocialAuth.NET and ASP.NET MVC integration**. Our next focus would be on roles.

As a note, we did try to use `[Authorize]` attribute with SocialAuth.NET MVC but we were not able to use it as it is without using a new attribute as we couldn't find an option to override existing Authorize behavior without creating a new attribute inheriting from it.

Like to learn from a demo... visit [here](http://code.google.com/p/socialauth-net/downloads/list)