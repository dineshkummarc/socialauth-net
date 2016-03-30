# Certificate Export Tutorial #

When STS is created using "ASP.NET Security Token Service Web Site", it uses STSTestCert ceritificate while generating FederationMetadata.xml. In a production environment you would ideally use a paid certificate.

However, if you have created STS service in testing environment (as is in [this](tutorial_creating_sts.md) tutorial), you may need to export STSTestCert certificate when needed by a consumer. For example, if you want to add SharePoint as a consumer of your STS, it needs the .cer file of this certificate.




Following tutorial is a step by step instruction on how you can export STSTestCert certificate.

1.	Click Start -> type "MMC.exe" in search box and hit enter to open Microsoft Management Console (MMC)

2.	From File menu, select "Add Remove Snap-in".

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc1.PNG


3.	**Select "Certificates"** from Available Snap-ins and press "Add"

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc2.PNG


4.	Clicking Add will open a prompt to confirm certificate type. Select "Computer Account" and click "Next" and in next window select "Finish".

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc3.PNG

5.	Click "OK" in "Add/Remove snap-in" window.

6.	Expand Certificates node then Personal node and select certificates under it. You should see STSTestCert on right panel.

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc4.PNG


7.	Right Click STSTestCert and select All Tasks ->Export from content menu to open Export Wizard.

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc5.PNG

8.	Click "Next" in first screen of wizard

9.	In next screen select "No, do not export the private key" (if already not selected) and click next

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc6.PNG

10.	Continue clicking "Next" in 3rd screen. Ensure "DER encoded binary X.509 (.CER) is selected

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc7.PNG

11.	In next screen of wizard, specify any path where you want to export certificate. For example, type "c:\temp\STSTestCert.cer" and click "Next"

> http://socialauth-net.googlecode.com/svn/wiki/images/mmc8.PNG

12.	Click "Finish" in final screen of wizard. You should see a prompt "The export was successful". Select "OK" in that prompt which would close wizard.

13.	Go to location "c:\temp\" (or any other location you specified in step 11) to get certificate.