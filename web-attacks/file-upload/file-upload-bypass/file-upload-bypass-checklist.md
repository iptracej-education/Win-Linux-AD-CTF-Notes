# File Upload Bypass Checklist

### File Upload Testing Check List

[https://www.aptive.co.uk/blog/unrestricted-file-upload-testing/](https://www.aptive.co.uk/blog/unrestricted-file-upload-testing/)

**Discovery:**

* [ ] &#x20;Identify File Upload Points

\
**Windows IIS Server Black List File Upload Bypass:**

* [ ] &#x20;Upload a file with the semi colon after the black listed extension, such as: <mark style="color:red;">**`shell.asp;.jpg`**</mark>
* [ ] &#x20;Upload a directory with the .asp extension, then name the script within the directory with a permitted file extension, example: <mark style="color:red;">**`folder.asp\file.txt`**</mark>
* [ ] &#x20;When serving PHP via IIS `> < and .` get converted back to `? * .`
* [ ] &#x20;Use characters that can replace files, example <mark style="color:red;">**`<<`**</mark><mark style="color:red;">**&#x20;**</mark><mark style="color:red;">**can replace**</mark><mark style="color:red;">**&#x20;**</mark><mark style="color:red;">**`web.config`**</mark>
* [ ] &#x20;Try using spaces or dots after characters, example: <mark style="color:red;">**`foo.asp..... .. . . .`**</mark>
* [ ] &#x20;Attempt to disclose information in an error message by uploading a file with forbidden characters within the filename such as: <mark style="color:red;">**`| > < * ?"`**</mark>

**Apache Windows Black List Bypass:**

* [ ] &#x20;Windows 8.3 feature allows short names to replace existing files, example: web.config could be replaced by web\~config.con or .htaccess could be replaced by HTACCE\~1
* [ ] &#x20;Attempt to upload a . file, if the upload function root is `/www/uploads/` it will create a file called uploads in the directory above.

**General Black List Bypass:**

* [ ] &#x20;Identify what characters are being filtered – use burp intruder to assess the insert points with a meta character list
* [ ] &#x20;Ensure your list contains uncommon file extension types such as <mark style="color:red;">**`.php5`**</mark><mark style="color:red;">**,**</mark><mark style="color:red;">**`.php3`**</mark><mark style="color:red;">**,**</mark><mark style="color:red;">**`.phtml`**</mark>
* [ ] &#x20;Test for flaws in the protection mechanism, if it’s stripping file names can this be abused? Example: <mark style="color:red;">**`shell.p.phpp`**</mark> if the app strips .php it could rename the extension back to .php
* [ ] &#x20;Try a null byte `%00` at various places within the file name, example: <mark style="color:red;">**`shell.php%00.jpg`**</mark>, <mark style="color:red;">**`shell.php%0delete0.jpg`**</mark> – observe how the application responds
* [ ] &#x20;Double extensions: if the application is stripping or renaming the extension – What happens if you give it two extensions? Example: <mark style="color:red;">**`shell.php.php`**</mark> or 4 extentions <mark style="color:red;">**`shell.txt.jpg.png.asp`**</mark>
* [ ] &#x20;Try long file names, example, <mark style="color:red;">**`supermassivelongfileeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeename.php`**</mark> apply other filter bypass techniques used in conjunction with long file names
* [ ] &#x20;Try <mark style="color:red;">**`test.asp\`**</mark>, <mark style="color:red;">**`test.asp.\`**</mark>
* [ ] &#x20;Can you upload the flash XSS payload, that is named as a .jpg
* [ ] &#x20;Try the previous technique but use PDF or Silverlight instead
* [ ] &#x20;Same again but attempt to abuse crossdomain.xml or clientaccesspolicy.xml files
* [ ] &#x20;Try using <mark style="color:red;">**encoding**</mark> to bypass blacklist filters, try URL, HTML, Unicode and double encoding
* [ ] &#x20;Combine all of the above bypass techniques
* [ ] &#x20;Try using an alternative HTTP Verb, try using POST instead of <mark style="color:red;">**PUT**</mark> or GET (or vice versa), you can enumerate the options using Burp Intruder and the HTTP Verbs payload list
* [ ] &#x20;Additionally, ensure all input points are fuzzed for various input validation failures such as, XSS, Command Injection, XPath, SQLi, LDAPi, SSJI

**Bypassing File Size Upload Checks:**

* [ ] &#x20;Use EXIF image file technique
* [ ] &#x20;Inject shell directly after image data within Burp request

**Techniques for Executing Uploaded Shells:**

* [ ] &#x20;Apache MIME Types: Attempt to upload a renamed file e.g. <mark style="color:red;">**`shell.php.jpg`**</mark> or <mark style="color:red;">**`shell.asp;.jpg`**</mark> and assess if the web server process the file by exploiting weak Apache MIME types
* [ ] &#x20;Null Byte: Try a null byte <mark style="color:red;">**`%00`**</mark> at the end of the file name or within such as: <mark style="color:red;">**`shell.php%0delete0.jpg`**</mark>– observe how the application responds
* [ ] &#x20;Can you upload dot files, if so can you upload a .htaccess file an abuse AddType: `AddType application/x-httpd-php .foo`
* [ ] &#x20;Be mindful of any processing to upload files – Example: Could command injection be used within a file name that will later be processed by a backend backup script?
* [ ] &#x20;Be mindful of any processing to upload files – If compressed files are permitted, does the application extract them or vice versa?

***

\
