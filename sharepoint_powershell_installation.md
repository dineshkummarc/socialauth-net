## Step 1 : Create Certificate object ##

Open SharePoint Management Studio from Program Files > Microsoft SharePoint 2010 Products > SharePoint 2010 Managment Shell

![http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint_management_shell.png](http://socialauth-net.googlecode.com/svn/wiki/images/sharepoint_management_shell.png)

Now execute following command in powershell and create an object of certificate

```
$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("FULL PATH OF CERTIFICATE IMPORTED IN STEP 1")
```

![http://socialauth-net.googlecode.com/svn/wiki/images/create-certificate.png](http://socialauth-net.googlecode.com/svn/wiki/images/create-certificate.png)

## Step 2 : Create Claim object ##

In the same command window create a new mapping object of our primary identifier claim. In this example we have used UPN but we can use emailid or any other identifier also

```
$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -IncomingClaimTypeDisplayName "UPN" -SameAsIncoming
```

![http://socialauth-net.googlecode.com/svn/wiki/images/identifier-primary-claim.png](http://socialauth-net.googlecode.com/svn/wiki/images/identifier-primary-claim.png)

## Step 3 : Create realm object ##

Create a realm object for storing realm value that is used for redirecting user in order to authenticate trust

```
$realm = "{server URL}/_trust/"
```

Example strings
"http://sharepoint2010.brickred.com/_trust/"
"http://localhost/_trust/"
"http://localhost:4321/_trust/"

![http://socialauth-net.googlecode.com/svn/wiki/images/realm-object.png](http://socialauth-net.googlecode.com/svn/wiki/images/realm-object.png)

## Step 4 : Create signinurl object ##

Create a signin url object for storing the STS default page value. Make sure that this is not the login.aspx page but the default.aspx page of STS provider.

```
$signinurl = "STS SERVER DEFAULT PAGE URL"
```

Example strings
"http://sts.brickred.com:4321/STS/Default.aspx"

![http://socialauth-net.googlecode.com/svn/wiki/images/signin-url.png](http://socialauth-net.googlecode.com/svn/wiki/images/signin-url.png)

## Step 5 : Create Trusted Root Authority ##

Create New trusted root authority that SharePoint can understand. Give any name to your authority and the certificate object.

```
New-SPTrustedRootAuthority -Name "Name of your provider" -Certificate $cert
```

In our example we have used "BrickRed STS" as our trusted root authority name

![http://socialauth-net.googlecode.com/svn/wiki/images/new-sptrustedRootProvider.png](http://socialauth-net.googlecode.com/svn/wiki/images/new-sptrustedRootProvider.png)

Once the trusted root authority is configured in SharePoint, we can check this in Central Admin under Security > Manage Trust

![http://socialauth-net.googlecode.com/svn/wiki/images/manage-trust.png](http://socialauth-net.googlecode.com/svn/wiki/images/manage-trust.png)

![http://socialauth-net.googlecode.com/svn/wiki/images/brickred-sts-sp.png](http://socialauth-net.googlecode.com/svn/wiki/images/brickred-sts-sp.png)

## Step 6 : Create Trusted Identity Provider ##

Now the time has come to create the Trusted Identity Provider in SharePoint. This will be created using the certificate, mapping, signinurl and identifier claim

```
$ap = New-SPTrustedIdentityTokenIssuer -Name "!BrickRed Social Auth" -Description "!BrickRed SocailAuth Security Token Service" -Realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map -SignInUrl $signinurl -IdentifierClaim $map.InputClaimType 
$ap.UseWReplyParameter = 1;
$ap.update();
```

In our example we have used "BrickRed Social Auth" as our provider name. wreply parameter is set to true so that reply be included during redirection to the Security Assertion Markup Language (SAML) login provider

![http://socialauth-net.googlecode.com/svn/wiki/images/new-sptrustedIdentityTokenIssuer.png](http://socialauth-net.googlecode.com/svn/wiki/images/new-sptrustedIdentityTokenIssuer.png)

## Step 7 : Add more claims ##

The Trusted Identity Token Provider is configured and created in SharePoint. Now add the rest of claims that your application needs for business processing. Social auth claims are added which are general to all the global providers like displayname, dataofbirth, name, email, gender.etc...

```
$ap =Get-SPTrustedIdentityTokenIssuer
$ap.ClaimTypes.add("http://schemas.microsoft.com/ws/2008/06/identity/claims/role");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/dateofbirth");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/uri");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress");
$ap.ClaimTypes.add("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender");

$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer

$map = New-SPClaimTypeMapping "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer

$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication" -IncomingClaimTypeDisplayName "Authentication" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer

$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname" -IncomingClaimTypeDisplayName "Display Name" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer

$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/dateofbirth" -IncomingClaimTypeDisplayName "DOB" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer

$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/uri" -IncomingClaimTypeDisplayName "PictureURL" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer

$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender" -IncomingClaimTypeDisplayName "Gender" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
$ap =Get-SPTrustedIdentityTokenIssuer


$map = New-SPClaimTypeMapping "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "E-Mail" -SameAsIncoming
Add-SPClaimTypeMapping -Identity $map -TrustedIdentityTokenIssuer $ap
$ap.update();
```


In our example we have used role, authentication, displayname, dateofbirth, uri, name, emailaddress and gender. Note that it is not necessary that all the claims will be returned by all the global providers. It strictly depends on user and provider settings.

![http://socialauth-net.googlecode.com/svn/wiki/images/add-claims.png](http://socialauth-net.googlecode.com/svn/wiki/images/add-claims.png)


OK, We are now done with all the powershell stuff.  If you are willing to execute all the powershell commands in one go then please download the powershell script file from [here](http://code.google.com/p/socialauth-net/downloads/detail?name=SharePoint-SocialAuth-Configuration-Script-0.1.zip)