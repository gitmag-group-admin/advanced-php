# PHP Advanced
 - PHP Date and Time
 - PHP Include
 - PHP  File Handling
 - PHP  File Upload
 - PHP  Cookies
 - PHP  Sessions
 - PHP  Filters
 - PHP  Callback Functions
 - PHP  Exceptions
 - PHP  And JSON

## PHP Date and Time
The PHP  `date()`  function is used to format a date and/or a time.

```php
date(format, timestamp);
```
- *Required. Specifies the format of the timestamp*
- *Optional. Specifies a timestamp. Default is the current date and time*

**Get a Time**
Here are some characters that are commonly used for times:

-   d - The day of the month (from 01 to 31)
-   m - A numeric representation of a month (from 01 to 12)
-   Y - A four digit representation of a year
-   y - A two digit representation of a year
-   F - A full textual representation of a month (January through December)
-   l - A full textual representation of a day
-   H - 24-hour format of an hour (00 to 23)
-   h - 12-hour format of an hour with leading zeros (01 to 12)
-   i - Minutes with leading zeros (00 to 59)
-   s - Seconds with leading zeros (00 to 59)
-   a - Lowercase Ante meridiem and Post meridiem (am or pm)

```php
echo  "The time is " . date("h:i:sa") . "\n";
echo  "The time is " . date("Y-m-d") . "\n";
echo  "The time is " . date("H-m-d H:i:s");
```

> Note that the PHP date() function will return the current date/time of the server!

**Get Your Time Zone**
If the time you got back from the code is not correct, it's probably because your server is in another country or set up for a different timezone.
So, if you need the time to be correct according to a specific location, you can set the timezone you want to use.
The example below sets the timezone to "America/New_York", then outputs the current time in the specified format:
```php
date_default_timezone_set("Asia/Tehran");  
echo  "The time is " . date("h:i:sa");
```

**Create a Date With mktime()**
The PHP `mktime()` function returns the Unix timestamp for a date. The Unix timestamp contains the number of seconds between the Unix Epoch (January 1 1970 00:00:00 GMT) and the time specified.
```php
mktime(hour, minute, second, month, day, year);
```
The example below creates a date and time with the  `date()`  function from a number of parameters in the  `mktime()`  function:
```php
$d = mktime(11, 14, 54, 7, 8, 2021);  
echo  "Created date is " . date("Y-m-d h:i:sa", $d);
```

**Create a Date From a String With strtotime()**
The PHP `strtotime()` function is used to convert a human readable date string into a Unix timestamp.
```php
strtotime(time, now);
```
The example below creates a date and time from the  `strtotime()`  function:
```php
$d = strtotime("10:30pm April 15 2021");  
echo  "Created date is " . date("Y-m-d h:i:sa", $d);
```
PHP is quite clever about converting a string to a date, so you can put in various values:
```php
$d = strtotime("tomorrow");  
echo date("Y-m-d h:i:sa", $d) . "\n";  
  
$d = strtotime("next Saturday");  
echo date("Y-m-d h:i:sa", $d) . "\n";  
  
$d = strtotime("+3 Months");  
echo date("Y-m-d h:i:sa", $d) . "\n";
```
**PHP  time()  Function**
Return the current time as a Unix timestamp, then format it to a date:
```php
$t = time();  
echo $t . "\n";  
echo date("Y-m-d",$t);
```
**More Date Examples**
The example below outputs the dates for the next six Saturdays:
```php
$startdate = strtotime("Saturday");  
$enddate = strtotime("+6 weeks", $startdate);  
  
while ($startdate < $enddate) {  
    echo date("M d", $startdate) . "\n";  
    $startdate = strtotime("+1 week", $startdate);  
}
```
The example below outputs the number of days until 4th of July:
```php
$d1 = strtotime("July 04");  
$d2 = ceil(($d1-time())/60/60/24);  
echo  "There are " . $d2 ." days until 4th of July.";
```

## PHP  Include Files
The  `include`  (or  `require`) statement takes all the text/code/markup that exists in the specified file and copies it into the file that uses the include statement.
Including files is very useful when you want to include the same PHP, HTML, or text on multiple pages of a website.

**PHP include and require Statements**
It is possible to insert the content of one PHP file into another PHP file (before the server executes it), with the include or require statement.

**The include and require statements are identical, except upon failure:**
-   `require`  will produce a fatal error (E_COMPILE_ERROR) and stop the script
-   `include`  will only produce a warning (E_WARNING) and the script will continue

```php
include 'filename';  

require 'filename';
```
**PHP include Examples**
Assume we have a standard footer file called "footer.php", that looks like this:
```php
<?php
echo "<p>Copyright &copy; 1999-" . date("Y") . " gitmag.ir</p>";  
?>
```
To include the footer file in a page, use the  `include`  statement:
```php
<html>  
<body>  
    <h1>Welcome to my home page!</h1>  
    <p>Some text.</p>  
    <p>Some more text.</p>  
    <?php  include  'footer.php';?>  
</body>  
</html>
```

Assume we have a file called "vars.php", with some variables defined:
```php
<?php
$color = 'red';  
$car = 'BMW';
?>
```
Then, if we include the "vars.php" file, the variables can be used in the calling file:
```php
<html>  
<body>  
    <h1>Welcome to my home page!</h1>  
    <?php  
    include  'vars.php';  
    echo  "I have a $color $car.";  
    ?>  
</body>  
</html>
```

**PHP include vs. require**
The  `require`  statement is also used to include a file into the PHP code.
However, there is one big difference between include and require; when a file is included with the `include` statement and PHP cannot find it, the script will continue to execute:
```php
<html>  
<body>  
    <h1>Welcome to my home page!</h1>  
    <?php
    include  'noFileExists.php';  
    echo  "I have a $color $car.";  
    ?>  
</body>  
</html>
```
If we do the same example using the  `require`  statement, the echo statement will not be executed because the script execution dies after the  `require`  statement returned a fatal error:
```php
<html>  
<body>  
    <h1>Welcome to my home page!</h1>  
    <?php
    require  'noFileExists.php';  
    echo  "I have a $color $car.";  
    ?>  
</body>  
</html>
```

> Use  `require`  when the file is required by the application. 
> Use  `include`  when the file is not required and application should continue when file is not found.

## PHP File Handling
File handling is an important part of any web application. You often need to open and process a file for different tasks.

**PHP Manipulating Files**
PHP has several functions for creating, reading, uploading, and editing files.

> **Be careful when manipulating files!**
> When you are manipulating files you must be very careful.
> You can do a lot of damage if you do something wrong. Common errors
> are: editing the wrong file, filling a hard-drive with garbage data,
> and deleting the content of a file by accident.

**PHP readfile() Function**
The  `readfile()`  function reads a file and writes it to the output buffer.
The PHP code to read the file and write it to the output buffer is as follows (the `readfile()` function returns the number of bytes read on success):
```php
echo readfile("file.txt");
```
The  `readfile()`  function is useful if all you want to do is open up a file and read its contents.

**PHP Open File - fopen()**
A better method to open files is with the  `fopen()`  function. This function gives you more options than the  `readfile()`  function.

The first parameter of  `fopen()`  contains the name of the file to be opened and the second parameter specifies in which mode the file should be opened. The following example also generates a message if the fopen() function is unable to open the specified file:
```php
$myfile = fopen("file.txt", "r");
echo fread($myfile, filesize("file.txt"));  
fclose($myfile);
```
> **Tip:**  The  `fread()`  and the  `fclose()`  functions will be explained below.

The file may be opened in one of the following modes:
| Modes | Description |
|--|--|
| r | Open a file for read only. |
| w | Open a file for write only. |
| a | Open and write end of file. |
| x | Creates a new file for write only. |
| r+ | Open a file for read/write. |
| w+ | Open a file for read/write. |
| a+ | Open a file for read/write end of file. |
| x+ | Creates a new file for read/write. |

**PHP Create File - fopen()**
The  `fopen()`  function is also used to create a file. Maybe a little confusing, but in PHP, a file is created using the same function used to open files.
If you use  `fopen()`  on a file that does not exist, it will create it, given that the file is opened for writing (w) or appending (a).
The example below creates a new file called "testfile.txt". The file will be created in the same directory where the PHP code resides:
```php
$myfile = fopen("testfile.txt", "w")
```

**PHP Write to File - fwrite()**
The  `fwrite()`  function is used to write to a file.
The first parameter of  `fwrite()`  contains the name of the file to write to and the second parameter is the string to be written.
The example below writes a couple of names into a new file called "newfile.txt":
```php
$myfile = fopen("newfile.txt", "w"); 
$txt = "shorab\n";  
fwrite($myfile, $txt);  
$txt = "sohrab\n";  
fwrite($myfile, $txt);  
fclose($myfile);
```
Notice that we wrote to the file "newfile.txt" twice. Each time we wrote to the file we sent the string $txt that first contained "John Doe" and second contained "Jane Doe". After we finished writing, we closed the file using the `fclose()` function.

If we open the "newfile.txt" file it would look like this:
```
sohrab 
sohrab
```
**PHP  copy()  Function**
The copy() function copies a file.
```php
copy(from_file, to_file, context);
```
Copy "source.txt" to "target.txt":
```php
echo copy("source.txt","target.txt");
```
- from_file:  Required. Specifies the path to the file to copy from
- to_file:  Required. Specifies the path to the file to copy to
- context:  Optional. Specifies a context resource created with stream_context_create()

**PHP  file_exists()  Function**
The file_exists() function checks whether a file or directory exists.
```php
file_exists(path);
```
- path:  Required. Specifies the path to the file or directory to check

Check whether a file exists:
```php
echo file_exists("test.txt");
```
**PHP  is_dir()  Function**
The is_dir() function checks whether the specified filename is a directory.
```php
is_dir(file);
```
- file: Required. Specifies the path to the file to check

```php
$file = "test";  
if(is_dir($file)) {  
    echo ("$file is a directory");  
} else {  
    echo ("$file is not a directory");  
}
```
**PHP  is_file()  Function**
The is_file() function checks whether the specified filename is a regular file.
```php
is_file(file);
```
- file: Required. Specifies the path to the file to check

Check whether the specified filename is a regular file:
```php
$file = "test.txt";  
if(is_file($file)) {  
    echo ("$file is a regular file");  
} else {  
    echo ("$file is not a regular file");  
}
```

**PHP  mkdir()  Function**
The mkdir() function creates a directory specified by a pathname.
```php
mkdir(path, mode, recursive, context);
```
- path:  Required. Specifies the directory path to create
- mode:  Optional. Specifies permissions. By default, the mode is 0777.
- recursive: Optional. Specifies if the recursive mode is set (added in PHP 5)
- context: Optional. Specifies the context of the file handle. Context is a set of options that can modify the behavior of a stream.

Create a directory named "test":
```php
mkdir("test");
```

**PHP  rename()  Function**
The rename() function renames a file or directory.
```php
rename(old, new, context);
```
- old:  Required. Specifies the file or directory to be renamed
- new:  Required. Specifies the new name for the file or directory
- context: Optional. Specifies the context of the file handle. Context is a set of options that can modify the behavior of a stream

Rename a directory + a file:
```php
rename("images","pictures");  
rename("/test/file1.txt","/home/docs/my_file.txt");
```

**PHP  unlink()  Function**
The unlink() function deletes a file.
```php
unlink(filename, context);
```
- filename:  Required. Specifies the path to the file to delete
- context: Optional. Specifies the context of the file handle. Context is a set of options that can modify the behavior of a stream

Delete a file:
```php
$file = fopen("test.txt","w");  
echo fwrite($file,"Hello World. Testing!");  
fclose($file);  
  
unlink("test.txt");
```

**PHP  rmdir()  Function**
The rmdir() function removes an empty directory.
```php
rmdir(dir, context);
```
- dir:  Required. Specifies the path to the directory to be removed.
- context:  Optional. Specifies the context of the file handle. Context is a set of options that can modify the behavior of a stream.

Remove "images" directory:
```php
$path = "images";  
if(!rmdir($path)) {  
    echo "Could not remove $path";  
}
```

## PHP  File Upload
With PHP, it is easy to upload files to the server.
However, with ease comes danger, so always be careful when allowing file uploads!

**Configure The "php.ini" File**
First, ensure that PHP is configured to allow file uploads.
In your "php.ini" file, search for the  `file_uploads`  directive, and set it to On:
```
file_uploads = On
```

**Create The HTML Form**
Next, create an HTML form that allow users to choose the image file they want to upload:
```html
<!DOCTYPE html>  
<html>  
<body>  
  
<form method="post" enctype="multipart/form-data">  
    Select image to upload:  
    <input type="file"  name="fileToUpload"  id="fileToUpload">  
    <br>
    <input type="submit"  value="Upload Image"  name="submit">  
</form>  
  
</body>  
</html>
```

Some rules to follow for the HTML form above:

-   *Make sure that the form uses method="post"*
-   *The form also needs the following attribute: enctype="multipart/form-data". It specifies which content-type to use when submitting the form*

Other things to notice:

-   *The type="file" attribute of the `<input>` tag shows the input field as a file-select control, with a "Browse" button next to the input control*
```php
array(
    "fileToUpload" => array(
	"name" => "channels4_profile.jpg",
	"type" => "image/jpeg",
	"tmp_name" => "C:\Users\Salar\AppData\Local\Temp\phpE31D.tmp",
	"error" => 0,
	"size" => 38069,
    )
)

```

**Create The Upload File PHP Script**
```php
if(isset($_POST["submit"])) { 
    $dir = "uploads/";  
    $name = $_FILES["fileToUpload"]["name"];   
    $check = move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $dir . $name);  
    if($check) {  
        echo  "$name is uploaded";  
    } else {  
        echo  "$name is not uploaded";  
    }  
}
```

>   **Note:** You will need to create a new directory called "uploads"in the directory where current file resides. The uploaded files will be saved there.

**Check if File Already Exists**
Now we can add some restrictions.

First, we will check if the file already exists in the "uploads" folder. If it does, an error message is displayed:
```php
$upload_ok = true;
// Check if file already exists  
if (file_exists($target_file)) {  
    echo "Sorry, file already exists.";
    $upload_ok = false;
}
```
**Limit File Size**
The file input field in our HTML form above is named "fileToUpload".

Now, we want to check the size of the file. If the file is larger than 500KB, an error message is displayed:
```php
// Check file size  
if ($_FILES["fileToUpload"]["size"] > 5000) {  
    echo "Sorry, your file is too large.";  
    $upload_ok = false;
}
```
**Limit File Type**
The code below only allows users to upload JPG, JPEG, PNG, and GIF files. All other file types gives an error message:
```php
// Allow certain file formats  
$type = strtolower(pathinfo($name, PATHINFO_EXTENSION));
if($type != "jpg" && $type != "png" && $type != "jpeg" && $type != "gif" ) {  
    echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.";
    $upload_ok = false;
}
```

**Complete Upload File PHP Script**
The complete file now looks like this:

```php
if(isset($_POST["submit"])) {

    $dir = "uploads/";  
    $name = $_FILES["fileToUpload"]["name"]; 
    $size =  $_FILES["fileToUpload"]["size"];
    $type = strtolower(pathinfo($name, PATHINFO_EXTENSION));
    $upload_ok = true;
    
    // Check if file already exists  
    if (file_exists($dir . $name)) {  
        echo  "Sorry, file already exists.";  
        $upload_ok = false;  
    }  
  
    // Check file size  
    if ($size > 5000) {  
        echo  "Sorry, your file is too large.";  
        $upload_ok = false;  
    }  
  
    // Allow certain file formats  
    if($type != "jpg" && $type != "png" && $type != "jpeg" && $type != "gif" ) {  
        echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.";
        $upload_ok = false;
    }

    if($upload_ok) {
        $check = move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $dir . $name);  
        if($check) {  
            echo  "$name is uploaded";  
        } else {  
            echo  "$name is not uploaded";  
        }
    }  
}
```

## PHP Cookies
A cookie is often used to identify a user. A cookie is a small file that the server embeds on the user's computer. Each time the same computer requests a page with a browser, it will send the cookie too. With PHP, you can both create and retrieve cookie values.

**Create Cookies With PHP**
A cookie is created with the `setcookie()` function.
```php
setcookie(name, value, expire, path, domain, secure, httponly);
setcookie(name, value, options) // php > 7
```
*Only the `name` parameter is required. All other parameters are optional.*

- *name*:  The name of the cookie.
- *value*:  The value of the cookie. This value is stored on the clients computer;
- *expires*:  The time the cookie expires. This is a Unix timestamp so is in number of seconds since the epoch.
- *path*:  The path on the server in which the cookie will be available on. If set to `'/'`, the cookie will be available within the entire `domain`.
- *domain*:  The (sub)domain that the cookie is available to. Setting this to a subdomain (such as `'www.example.com'`) will make the cookie available to that subdomain and all other sub-domains of it.
- *secure*:  Indicates that the cookie should only be transmitted over a secure HTTPS connection from the client. When set to **`true`**, the cookie will only be set if a secure connection exists.
- *httponly*:  When **`true`** the cookie will be made accessible only through the HTTP protocol. This means that the cookie won't be accessible by scripting languages, such as JavaScript.


**PHP Create/Retrieve a Cookie**
The following example creates a cookie named "user" with the value "John Doe". The cookie will expire after 30 days (86400 * 30). The "/" means that the cookie is available in entire website (otherwise, select the directory you prefer).

We then retrieve the value of the cookie "user" (using the global variable $_COOKIE). We also use the `isset()` function to find out if the cookie is set:
```php
<?php  
$cookie_name = "user";  
$cookie_value = "John Doe";  
setcookie($cookie_name, $cookie_value, time() + (86400 * 30), "/"); // 86400 = 1 day  
?>  
<html>  
<body>  
  
<?php  
if(!isset($_COOKIE[$cookie_name])) {  
    echo "Cookie named '" . $cookie_name . "' is not set!";  
} else {  
    echo "Cookie '" . $cookie_name . "' is set!<br>";  
    echo "Value is: " . $_COOKIE[$cookie_name];  
}  
?>  
  
</body>  
</html>
```

> ****Note:**** The `setcookie()` function must appear BEFORE the `<html>` tag.

**Modify a Cookie Value**
To modify a cookie, just set (again) the cookie using the `setcookie()` function:
```php
<?php  
$cookie_name = "user";  
$cookie_value = "Alex Porter";  
setcookie($cookie_name, $cookie_value, time() + (86400 * 30), "/");  
?>  
<html>  
<body>  
  
<?php  
if(!isset($_COOKIE[$cookie_name])) {  
    echo "Cookie named '" . $cookie_name . "' is not set!";  
} else {  
    echo "Cookie '" . $cookie_name . "' is set!<br>";  
    echo "Value is: " . $_COOKIE[$cookie_name];  
}  
?>  
  
</body>  
</html>
```
**Delete a Cookie**
To delete a cookie, use the `setcookie()` function with an expiration date in the past:
```php
<?php  
// set the expiration date to one hour ago  
setcookie("user", "", time() - 3600);  
?>  
<html>  
<body>  
  
<?php  
echo "Cookie 'user' is deleted.";  
?>  
  
</body>  
</html>
```

**Check if Cookies are Enabled**
The following example creates a small script that checks whether cookies are enabled. First, try to create a test cookie with the `setcookie()` function, then count the $_COOKIE array variable:

```php
<?php  
setcookie("test_cookie", "test", time() + 3600, '/');  
?>  
<html>  
<body>  
  
<?php  
if(count($_COOKIE) > 0) {  
    echo "Cookies are enabled.";  
} else {  
    echo "Cookies are disabled.";  
}  
?>  
  
</body>  
</html>
```
## PHP Sessions
A session is a way to store information (in variables) to be used across multiple pages.
Unlike a cookie, the information is not stored on the users computer.

When you work with an application, you open it, do some changes, and then you close it. This is much like a Session. The computer knows who you are. It knows when you start the application and when you end. But on the internet there is one problem: the web server does not know who you are or what you do, because the HTTP address doesn't maintain state.

Session variables solve this problem by storing user information to be used across multiple pages (e.g. username, favorite color, etc). By default, session variables last until the user closes the browser.

So; Session variables hold information about one single user, and are available to all pages in one application.

> **Tip:** If you need a permanent storage, you may want to store the data in a database

## Start a PHP Session
A session is started with the `session_start()` function.

Session variables are set with the PHP global variable: $_SESSION.

Now, let's create a new page called "index.php". In this page, we start a new PHP session and set some session variables:

```php
<?php  
// Start the session  
session_start();  
?>  
<!DOCTYPE html>  
<html>  
<body>  
  
<?php  
    // Set session variables  
    $_SESSION["favcolor"] = "green";  
    $_SESSION["favanimal"] = "cat";  
    echo "Session variables are set.";  
?>  
  
</body>  
</html>
```

> ****Note:**** The `session_start()` function must be the very first thing in your document. Before any HTML tags.

**Get PHP Session Variable Values**
Next, we create another page called "test.php". From this page, we will access the session information we set on the first page ("index.php").

Notice that session variables are not passed individually to each new page, instead they are retrieved from the session we open at the beginning of each page (`session_start()`).

Also notice that all session variable values are stored in the global $_SESSION variable:
```php
<?php  
session_start();  
?>  
<!DOCTYPE html>  
<html>  
<body>  
  
<?php  
// Echo session variables that were set on previous page  
echo "Favorite color is " . $_SESSION["favcolor"] . ".<br>";  
echo "Favorite animal is " . $_SESSION["favanimal"] . ".";  
?>  
  
</body>  
</html>
```

> **How does it work? How does it know it's me?**  
> Most sessions set a user-key on the user's computer that looks something like this:765487cf34ert8dede5a562e4f3a7e12. Then, when a session is opened on another page, it scans the computer for a user-key. If there is a match, it accesses that session, if not, it starts a new session.

**Modify a PHP Session Variable**
To change a session variable, just overwrite it:

```php
<?php  
session_start();  
?>  
<!DOCTYPE html>  
<html>  
<body>  
  
<?php  
// to change a session variable, just overwrite it  
$_SESSION["favcolor"] = "yellow";  
print_r($_SESSION);  
?>  
  
</body>  
</html>
```

**Destroy a PHP Session**
To remove all global session variables and destroy the session, use  `session_destroy()`:

```php
<?php  
session_start();  
?>  
<!DOCTYPE html>  
<html>  
<body>  
  
<?php  
// destroy the session  
session_destroy();  
?>  
  
</body>  
</html>
```

## PHP Filters
Validating data = Determine if the data is in proper form.
Sanitizing data = Remove any illegal character from the data.

**The PHP Filter Extension**
PHP filters are used to validate and sanitize external input.

The PHP filter extension has many of the functions needed for checking user input, and is designed to make data validation easier and quicker.

The  `filter_list()`  function can be used to list what the PHP filter extension offers:
```php
<table>  
    <tr>  
	<td>Filter Name</td>  
	<td>Filter ID</td>  
    </tr>  
    <?php  
    foreach  (filter_list()  as  $id =>$filter) {  
        echo  '<tr><td>'  . $filter .  '</td><td>'  . filter_id($filter) .  '</td></tr>';  
    }  
?>  
</table>
```

**Why Use Filters?**
Many web applications receive external input. External input/data can be:
-   User input from a form
-   Cookies
-   Web services data
-   Server variables
-   Database query results

> You should always validate external data! 
> Invalid submitted data can lead to security problems and break your webpage!  
> By using PHP filters you can be sure your application gets the correct input!

**PHP filter_var() Function**
The  `filter_var()`  function both validate and sanitize data.
The  `filter_var()`  function filters a single variable with a specified filter. It takes two pieces of data:
-   The variable you want to check
-   The type of check to use

**Sanitize a String**
The following example uses the  `filter_var()`  function to remove all HTML tags from a string:
```php
$str = "<h1>Hello World!</h1>";  
$newstr = filter_var($str, FILTER_SANITIZE_STRING);  
echo $newstr;
```
**Validate an IP Address**
The following example uses the  `filter_var()`  function to check if the variable $ip is a valid IP address:
```php
$ip = "127.0.0.1";  
  
if (filter_var($ip, FILTER_VALIDATE_IP) === false) {
    echo("$ip is not a valid IP address");  
} else {  
    echo("$ip is a valid IP address"); 
}
```
**Sanitize and Validate an Email Address**
The following example uses the  `filter_var()`  function to first remove all illegal characters from the $email variable, then check if it is a valid email address:
```php
$email = "info.gitmag.group@example.com";  
  
// Remove all illegal characters from email  
$email = filter_var($email, FILTER_SANITIZE_EMAIL);  
  
// Validate e-mail  
if (filter_var($email, FILTER_VALIDATE_EMAIL) === false) {  
    echo("$email is not a valid email address");  
} else {  
    echo("$email is a valid email address");  
}
```
**Validate a URL**
The following example uses the  `filter_var()`  function to first remove all illegal characters from a URL, then check if $url is a valid URL:
```php
$url = "https://www.gitmag.ir";  

// Validate url  
if (filter_var($url, FILTER_VALIDATE_URL) === false) {  
    echo("$url is not a valid URL");  
} else { 
    echo("$url is a valid URL");  
}
```

## PHP Callback Functions
A callback function (often referred to as just "callback") is a function which is passed as an argument into another function.

Any existing function can be used as a callback function. To use a function as a callback function, pass a string containing the name of the function as the argument of another function:

Pass a callback to PHP's  `array_map()`  function to calculate the length of every string in an array:
```php
function my_callback($item) {  
    return strlen($item);  
}  
  
$array = ["apple", "orange", "banana", "coconut"];  
$lengths = array_map("my_callback", $array);  
print_r($lengths);
```
Use an anonymous function as a callback for PHP's  `array_map()`  function:
```php
$array = ["apple", "orange", "banana", "coconut"];  
$lengths = array_map( function($item) { return strlen($item); } , $array);  
print_r($lengths);
```
The `array_filter()` function filters the values of an array using a callback function.
```php
function check($item) {  
    return ($item % 2) == 0;  
}  
  
$array = array(2, 5, 6, 12, 17);  
$result = array_filter($array, "check");
print_r($result);
```

**Callbacks in User Defined Functions**
User-defined functions and methods can also take callback functions as arguments. To use callback functions inside a user-defined function or method, call it by adding parentheses to the variable and pass arguments as with normal functions:
```php
function exclaim($str) {  
    return $str . "! ";  
}  
  
function ask($str) {  
    return $str . "? ";  
}  
  
function  printFormatted($str, $format) {  
    // Calling the $format callback function  
    echo $format($str);  
}  
  
// Pass "exclaim" and "ask" as callback functions to printFormatted()  
printFormatted("Hello world", "exclaim");  
printFormatted("Hello world", "ask");
```

## PHP  Exceptions
An exception is an object that describes an error or unexpected behaviour of a PHP script.
User defined functions and classes can also throw exceptions.
Exceptions are a good way to stop a function when it comes across data that it cannot use.

**Throwing an Exception**
The  `throw`  statement allows a user defined function or method to throw an exception. When an exception is thrown, the code following it will not be executed.

If an exception is not caught, a fatal error will occur with an "Uncaught Exception" message.
Lets try to throw an exception without catching it:

```php
function divide($dividend, $divisor) {  
    if($divisor == 0) {  
	throw  new Exception("Division by zero");  
    }  
    return $dividend / $divisor;  
}  
  
echo divide(5, 0);
```
The result will look something like this:
```
Fatal error: Uncaught Exception: Division by zero in C:\webfolder\test.php:4  
Stack trace: #0 C:\webfolder\test.php(9):  
divide(5, 0) #1 {main} thrown in **C:\webfolder\test.php** on line **4**
```

**The try...catch Statement**
To avoid the error from the example above, we can use the  `try...catch`  statement to catch exceptions and continue the process.
```php
try {  
    code that can throw exceptions  
} catch(Exception $e) {  
    code that runs when an exception is caught  
}
```
Show a message when an exception is thrown:
```php
function divide($dividend, $divisor) {  
    if($divisor == 0) {  
	throw  new Exception("Division by zero");  
    }  
    return $dividend / $divisor;  
}  
  
try {  
    echo  divide(5, 0);  
} catch(Exception $e) {  
    echo  "Unable to divide.";  
}
```
The catch block indicates what type of exception should be caught and the name of the variable which can be used to access the exception. In the example above, the type of exception is `Exception` and the variable name is `$e`.

**The try...catch...finally Statement**
The  `try...catch...finally`  statement can be used to catch exceptions. Code in the  `finally`  block will always run regardless of whether an exception was caught. If  `finally`  is present, the  `catch`  block is optional.
```php
try {  
    code that can throw exceptions  
} catch(Exception $e) {  
    code that runs when an exception is caught  
} finally {  
    code that always runs regardless of whether an exception was caught  
}
```
Show a message when an exception is thrown and then indicate that the process has ended:
```php
function divide($dividend, $divisor) {  
    if($divisor == 0) {  
	throw  new Exception("Division by zero");  
    }  
    return $dividend / $divisor;  
}  
  
try {  
    echo  divide(5, 0);  
} catch(Exception $e) {  
    echo  "Unable to divide. ";  
} finally {  
    echo  "Process complete.";  
}
```
Output a string even if an exception was not caught:
```php
function divide($dividend, $divisor) {  
    if($divisor == 0) {  
	throw  new Exception("Division by zero");  
    }  
    return $dividend / $divisor;  
}  
  
try {  
    echo divide(5, 0);  
} finally {  
    echo  "Process complete.";  
}
```

## PHP and JSON

**What is JSON?**
JSON stands for JavaScript Object Notation, and is a syntax for storing and exchanging data.
Since the JSON format is a text-based format, it can easily be sent to and from a server, and used as a data format by any programming language.

**PHP and JSON**
PHP has some built-in functions to handle JSON.
First, we will look at the following two functions:
-   json_encode()
-   json_decode()

**PHP - json_encode()**
The  json_encode()  function is used to encode a value to JSON format.
```php
$age = ["Peter" => 35, "Ben" => 37, "Joe" => 43];  
  
echo json_encode($age);
```

**PHP - json_decode()**
The  json_decode()  function is used to decode a value to php array or object.
```php
$age = '{"Peter": 35, "Ben": 37, "Joe": 43}';  
  
print_r(json_decode($age));
```
