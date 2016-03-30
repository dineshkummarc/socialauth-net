## Step 1 : Create SharePoint 2010 Web Application ##
Its now the turn to do some configuration at SharePoint Side. First Create a web application that has claims based authentication

![http://socialauth-net.googlecode.com/svn/wiki/images/Create-new-web-application.png](http://socialauth-net.googlecode.com/svn/wiki/images/Create-new-web-application.png)

In our example we have created a claims based authentication site on port 4321. We are using both Windows as well as "BrickRed Social Auth" claims for authentication. This process takes couple of minutes and once provisioned it will ask user to create site collection

![http://socialauth-net.googlecode.com/svn/wiki/images/webapplication-application-created.png](http://socialauth-net.googlecode.com/svn/wiki/images/webapplication-application-created.png)


## Step 2 : Create Site Collection ##

Now create a site collection based on your requirements.

![http://socialauth-net.googlecode.com/svn/wiki/images/create-site-collection.png](http://socialauth-net.googlecode.com/svn/wiki/images/create-site-collection.png)

In our example we have created a team site.

## Step 3 : Provide Access to socialauth users ##

Login with Windows Authentication Administrator Account and give view access to the role "socialauth" so that all the authenticated users can login into the site. We can use Authentication claim value to identify the provider name like Facebook, Twitter, Google etc...

![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-windows-login.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-windows-login.png)

Go to Site permissions

![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-site-permissions.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-site-permissions.png)

Now add users that has a role "socialauth" to view group
![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-grant-permission.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-grant-permission.png)
![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-add-user.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-add-user.png)
![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-add-socialauth-role.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-add-socialauth-role.png)
![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-add-socialauth-role-view-permission.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-add-socialauth-role-view-permission.png)
![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-view-permission.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint-view-permission.png)

