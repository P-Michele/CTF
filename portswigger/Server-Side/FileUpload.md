# How to prevent file upload vulnerabilities

Check the file extension against a whitelist of permitted extensions
Make sure that the filename dosn't contain any substring that may create a path traversal
Rename uploaded files
Use an established framework for preprocessing file uploads
Don't upload files to the server filesystem before validate them

# How file works in HTTP:

If the file type is executable, such as a PHP file, and the server is configured to execute files of this type, it will assign variables based on the headers and parameters in the
HTTP request before running the script. The resulting output may then be sent to the client in an HTTP response.

If the file type is executable, but the server is not configured to execute files of this type, it will generally respond with an error. However, in some cases, the contents of the
file may still be served to the client as plain text

You can watch the Content-Type Header to know what kind of file the server thinks it has served.

## Payloads and Bypass:

```
<?php echo file_get_contents('/path'); ?> You can visit the file to see the output of the path

<?php echo system($_GET['command']); ?>  you can inject command via URL visiting the file (GET /example/exploit.php?command=id HTTP/1.1)
```

-If the server trust **Content-Type** header, you can change it to upload your own file.

-Some server give an error code if you submit a file that they are not configured to execute. This is interesting 
because it can leak the source code sometimes. 

-Config to execute only php file in apache2 is :
```
etc/apache2/apache2.conf:

LoadModule php_module /usr/lib/apache2/modules/libphp.so
AddType application/x-httpd-php .php
```
-You can try to add your own rule. Normally you can't upload a **.htaccess** or **web.config** file to make directory-specific configuration.
But, if the web application fail to invlaidate your HTTP request, you can try to upload your own configuration file.

-Obfuscating file extensions:
```
exploit.pHp
exploit%2Ephp
exploit.asp;.jpg
exploit.asp%00.jpg
```
Try using multibyte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalization:
`xC0 x2E, xC4 xAE or xC0 xAE` may be translated to x2E if the filename parsed as a UTF-8 string, but then converted to ASCII characters before being used in a path. 

-Try to change magic bytes of the file.(

-Upload file with Race Condition:

Some websites upload the file directly to the main filesystem and then remove it again if it doesn't pass validation.
This kind of behavior is typical in websites that rely on anti-virus software and the like to check for malware. 
This may only take a few milliseconds, but for the short time that the file exists on the server you can try to execute it.

Similar race conditions can occur in functions that allow you to upload a file by providing a URL. 
In this case, the server has to fetch the file over the internet and create a local copy before it can perform any validation.

The race condition paylaod depends on the way the developer implement is own "sandbox". You will probably need to request the file when is checked from the antivirus.
To do so you need to knwow the path where the sandbox is located at.

-If you can upload HTML file or SVG file, you can try to upload a <script> to create a Stored XSS.

-Some server may implement PUT by default. You can try to upload a file wiht PUT even if it is not present in the web interface.





