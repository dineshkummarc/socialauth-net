# Introduction #

Trying SocialAuth.NET MVC demo is very easy. Following steps provide information on how to use this demo.


# Details #

  1. Download [SocialAuth 2.3 package](http://code.google.com/p/socialauth-net/downloads/list), if you haven't yet!
  1. Extract and copy MVC3Demo folder at your desired location
  1. Right Click folder and uncheck ReadOnly if it appears checked
  1. Open IIS and under default web site, create a new virtual directory MVC3Demo and point that virtual directory to aforesaid folder<br><img src='http://socialauth-net.googlecode.com/svn/wiki/images/mvcdemo_iis.png' /><br><br><br><a href='http://socialauth-net.googlecode.com/svn/wiki/images/mvcdemo_iis2.PNG'>http://socialauth-net.googlecode.com/svn/wiki/images/mvcdemo_iis2.PNG</a><br>
<ol><li>Ensure app pool is bind with 4.0 framework of .NET<br>
</li><li>Open your host file C:\Windows\System32\drivers\etc\hosts in notepad and add following entry (if you haven't added before)<br>
127.0.0.1 opensource.brickred.com</li></ol>

That's it!!<br>
Just Press F5 and view demo.<br>
<br>
<br>
<h1>Troubleshooting</h1>

<ol><li>Pressing F5 doesn't starts application<br><br>Try opening demo application using <a href='http://opensource.brickred.com/mvc3demo'>http://opensource.brickred.com/mvc3demo</a>. If you see an error related with can't access config file, it is likely because your folder doen't have permission to IIS_IUSRS. Refer this thread for help <a href='http://stackoverflow.com/questions/5615296/cannot-read-configuration-file-due-to-insufficient-permissions'>http://stackoverflow.com/questions/5615296/cannot-read-configuration-file-due-to-insufficient-permissions</a>. <br> If you see a different error like page not found, it is likely because either you have missed host entry or application is not properly configured in IIS.<br>
</li><li>Error: cannot be opened because it is version 655. This server supports version 612 and earlier. A downgrade path is not supported.<br> If you encounter this error or similar database error, please do following:<br>1. Delete all files in App_Data (it will regenrate automatically next time when you rin application)<br> 2. Ensure connection string points a to a valid sql server/express on your local machine