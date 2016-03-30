# Configure SharePoint to consume SocialAuth STS #

## Step 1 : Import STS Certificate ##

Get Certificate file from STS provider. Check step by step guide [here](tutorial_export_certificate.md)

## Step 2 : Powershell  Configurations ##

Follow [this](sharepoint_powershell_installation.md) step by step guide for all powershell configurations.

## Step 3 : SharePoint Configurations ##

Follow [this](sharepoint_server_configuration.md) step by step guide for SharePoint configurations.

## Step 4 : Results - Login with social auth ##

We are done with all the changes in SharePoint environment and ready for our social auth provider and claims based authentication is ready for use. Now Login with BrickRed social Auth provider

![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-signin.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-signin.png)

![http://socialauth-net.googlecode.com/svn/wiki/images/sts-login-screen.png](http://socialauth-net.googlecode.com/svn/wiki/images/sts-login-screen.png)

In this example I am using Facebook login. Clicking on Facebook will take me to facebook site for login
![http://socialauth-net.googlecode.com/svn/wiki/images/facebook-login.png](http://socialauth-net.googlecode.com/svn/wiki/images/facebook-login.png)

Hurray!!! We are able to login in SharePoint 2010 through Facebook. Right now we are using UPN but we can use email address or any other claim. Once this is done then we can update this users profile with his display name.
![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-facebook-login-success.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-facebook-login-success.png)