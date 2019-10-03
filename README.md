# FileURL
## Creating URLs for use in E-Mail Attachments
Why you should use URLs in E-mail messages
Attaching files to e-mail messages is a convenient way of distributing files to the
recipients listed on the address line. This is fine for one or just a few recipients,
however many attachments wind up getting sent to mailing list aliases that address
large numbers of recipients. There are a few problems with attaching files to e-mails
destined to multiple recipients:
*   Network and CPU bandwidth needed to send individual copies of attachments to each
addressee's mail server.
*   Disk space needed at mail servers to store individual copies of attachments to each
addressee.
*   Network and CPU bandwidth needed to download individual copies of attachments to each
addressee's mail client.
*   Disk space needed at file servers to store individual copies of attachments downloaded
by each addressee. A method of dealing with this problem manually is to create a Universal
Resource Locator (URL) and place the URL into the e-mail text

## Why using URLs in e-mails hasn't gained widespread acceptance (yet?)
The problem with manually creating URLs is it is a time consuming and error prone chore
for the sender. While manually creating URLs saves network and CPU bandwidth along with
disk space, it actually consumes the time of the sender, and is error prone. A simple
mistake like having the file permission set wrong on a URL can cause the recipients to
waste time asking the sender to correct the problem etc. There is simply no incentive for
the sender to use URLs.
## Automatic URL generation
A way to reduce the burden of creating URLs for a sender is to have a method of
automatically generating URLs to a file with properly set permissions. This is the
approach taken by the shell script **<tt>url</tt>**.
*   Generate a URL file for any given source file.
*   The URL file has the same name as the source file with a <tt>.http</tt> extension
appended to it.
*   The URL file is stored in the same directory as the source file so it can be
attached as easily to an e-mail message as the source file.
*   The URL file points to a copy of the source file with a unique name so only
recipients of that particular URL file attachment can see the file.
This prevents an earlier recipient from being able to see changes made to the source file
by dint of knowing a previous URL to an earlier version of the file. (A Security Feature)
*   The copy of the source file pointed at by the URL is contained in a directory that
cannot be listed, so a recipient of one URL cannot use the path provided to peruse other
files you have sent with <tt>FileURL</tt>. (A Security Feature)
*   The sender of the URL file still has control of the file in the directory and can
delete it if it becomes obsolete to prevent any additional accesses to the file.
Here's how it works:
<pre>$ cat hello_world.txt
Hello World!
$ url hello_world.txt
http://nfsweb.example.com/home/dnygren/public_html/tmp/2019-05-10_145004.hello_world.txt
</pre>
The link above is what you share with others. Below are some more details.
<pre>$ ls -la hello
*-rw------- 1 nygren uucp 13 May 10 14:49 hello_world.txt
-rw------- 1 nygren uucp 87 May 10 14:50 hello_world.txt.http
$ cat hello_world.txt.http
http://nfsweb.example.com/home/dnygren/public_html/tmp/2019-05-10_145004.hello_world.txt
$ url -h
Usage: url copies a specified file into a directory where it can be
publicly accessed with either a date and time stamp prefix or a user provided
filename. An http extension file that contains the URL to that file is placed
in the original directory.
Examples:
$ url SOURCE_FILE [DEST_FILE_NAME]
$ url backup.tar.gz (Creates 2019-05-10_134353.backup.tar.gz)
$ url file.bin file.bin.BugFix1234567 (Creates file.bin.BugFix1234567)
$ url -x (Exclude creation of .http file )
$ url -h (prints out this help message)
</pre>
A command line example is as follows:
<pre>
% url big_file_that_should_not_be_emailed_to_an_alias
http://nfsweb.example.com/home/nygren/public_html/tmp/2019-05-10_145004.big_file_that_should_not_be_emailed_to_an_alias
</pre>
## Installation Instructions
1.  Create a temporary directory under your home directory called public_html/tmp:
<tt>% mkdir ~/public_html/tmp</tt>
2.  Set the permissions so it is accessible but not readable to others:
<tt>% chmod 711 ~/public_html/tmp</tt>
3.  Copy <tt>url</tt> to a directory in your path (e.g. ~/bin).
4.  Set the permissions so it is executable by yourself:
<tt>% chmod 700 ~/bin/url</tt>
5.  Edit the url script so it knows where the file storage directory is and how to get to
it via http:
*   Set the file system path to a public directory to copy the file into.
*   <tt>URL_DIRECTORY=/net/n/home/nygren/public_html/tmp</tt>
*   Set corresponding http path to same public directory Intranet-only NFS to web server
*   <tt>HTTP_PREFIX=http://nfsweb.example.com/home/$LOGNAME/public_html/tmp</tt>
