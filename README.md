# This is a README file giving details about the project deploying the Item Catalog project to a Linux Server:

IP:18.197.102.169
SSH port: 2200
URL: http://18.197.102.169/ # Still haven't given it a domain name but will do

After setting up the server in Amazon Lightsail I had to take care of the ports and the users, 
thus creating a controlled environment.
Having done this, I started working on setting up a web server which in this case is a Apache2 server. 
After installing it, I had to 
configure its URLs and ports as well and run it. 

Install Sqlite:
sudo apt-get install sqlite3 libsqlite3-dev

Install WSGI:

Prerequisites:

	sudo apt-get update
	sudo apt-get install python libexpat1 

And the module itself:
	sudo apt-get update
	sudo apt-get install apache2 apache2-utils ssl-cert
	sudo apt-get install libapache2-mod-wsgi
	
After installation, apache2 needs to be restarted

	sudo systemctl restart apache2
	
And WSGI needs to be set up:
	The purpose of the WSGI specification is to provide a common mechanism for hosting 
	a Python web application on a range of different web servers supporting the Python 
	programming language.

A very simple WSGI application, and the one which should be used for the examples in this 
document, is as follows:

def application(environ, start_response):
    status = '200 OK'
    output = b'Hello World!'

    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
This sample application will need to be placed into what will be referred to as the WSGI 
application script file. For the examples presented here, the WSGI application will be 
run as the user that Apache runs as. As such, the user that Apache runs as must have read 
access to both the WSGI application script file and all the parent directories that contain it.

Note that mod_wsgi requires that the WSGI application entry point be called ‘application’. 
If you want to call it something else then you would need to configure mod_wsgi explicitly 
to use the other name. Thus, don’t go arbitrarily changing the name of the function. If you do,
 even if you set up everything else correctly the application will not be found.

Mounting The WSGI Application
There are a number of ways that a WSGI application hosted by mod_wsgi can be mounted against a specific URL.
 These methods are similar to how one would configure traditional CGI applications.

The main approach entails explicitly declaring in the main Apache configuration file the URL 
mount point and a reference to the WSGI application script file. In this case the mapping is 
fixed, with changes only being able to be made by modifying the main Apache configuration and 
restarting Apache.

When using mod_cgi to host CGI applications, this would be done using the ScriptAlias directive. 
For mod_wsgi, the directive is instead called WSGIScriptAlias:

WSGIScriptAlias /myapp /usr/local/www/wsgi-scripts/myapp.wsgi
This directive can only appear in the main Apache configuration files. The directive can be used
 at server scope but would normally be placed within the VirtualHost container for a particular
  site. It cannot be used within either of the Location, Directory or Files container directives,
   nor can it be used within a “.htaccess” file.

The first argument to the WSGIScriptAlias directive should be the URL mount point for the WSGI 
application. For this case the URL should not contain a trailing slash. The only exception to 
this is if the WSGI application is to be mounted at the root of the web server, in which case 
‘/’ would be used.

The second argument to the WSGIScriptAlias directive should be an absolute pathname to the WSGI
 application script file. It is into this file that the sample WSGI application code should be placed.

Note that an absolute pathname must be used for the WSGI application script file supplied as 
the second argument. It is not possible to specify an application by Python module name alone.
 A full path is used for a number of reasons, the main one being so that all the Apache access 
 controls can still be applied to indicate who can actually access the WSGI application.

Because the Apache access controls will apply, if the WSGI application is located outside of any
 directories already configured to be accessible to Apache, it will be necessary to tell Apache 
 that files within that directory can be used. To do this the Directory directive must be used:

<Directory /usr/local/www/wsgi-scripts>
<IfVersion < 2.4>
    Order allow,deny
    Allow from all
</IfVersion>
<IfVersion >= 2.4>
    Require all granted
</IfVersion>
</Directory>
Note that it is highly recommended that the WSGI application script file in this case
 NOT be placed within the existing DocumentRoot for your main Apache installation, or 
 the particular site you are setting it up for. This is because if that directory is 
 otherwise being used as a source of static files, the source code for your application 
 might be able to be downloaded.

You also should not use the home directory of a user account, as to do that would mean 
allowing Apache to serve up any files in that account. In this case any misconfiguration 
of Apache could end up exposing your whole account for downloading.

It is thus recommended that a special directory be setup distinct from other directories
 and that the only thing in that directory be the WSGI application script file, 
 and if necessary any support files it requires.

A complete virtual host configuration for this type of setup would therefore be something like:

<VirtualHost *:80>

    ServerName www.example.com
    ServerAlias example.com
    ServerAdmin webmaster@example.com

    DocumentRoot /usr/local/www/documents

    <Directory /usr/local/www/documents>
    <IfVersion < 2.4>
        Order allow,deny
        Allow from all
    </IfVersion>
    <IfVersion >= 2.4>
        Require all granted
    </IfVersion>
    </Directory>

    WSGIScriptAlias /myapp /usr/local/www/wsgi-scripts/myapp.wsgi

    <Directory /usr/local/www/wsgi-scripts>
    <IfVersion < 2.4>
        Order allow,deny
        Allow from all
    </IfVersion>
    <IfVersion >= 2.4>
        Require all granted
    </IfVersion>
    </Directory>

</VirtualHost>
After appropriate changes have been made Apache will need to be restarted. For this example, 
the URL ‘http://www.example.com/myapp’ would then be used to access the the WSGI application.

Note that you obviously should substitute the paths and hostname with values appropriate for your system.

I set up a virtual environment located in /var/www/itemCatalog/venv.

Here is a list of the required packages on my virtual environment running on the Amazon server:

bleach==3.1.0
certifi==2019.3.9
chardet==3.0.4
Click==7.0
Flask==1.0.3
Flask-HTTPAuth==3.3.0
Flask-SQLAlchemy==2.4.0
httplib2==0.12.3
idna==2.8
itsdangerous==1.1.0
Jinja2==2.10.1
MarkupSafe==1.1.1
oauth2client==4.1.3
packaging==19.0
passlib==1.7.1
postgres==2.2.2
psycopg2-binary==2.8.2
pyasn1==0.4.5
pyasn1-modules==0.2.5
pyparsing==2.4.0
requests==2.22.0
rsa==4.0
six==1.12.0
SQLAlchemy==1.3.3
uritemplate==3.0.0
urllib3==1.25.2
uWSGI==2.0.18
webencodings==0.5.1
Werkzeug==0.15.4

====

Made use of third party sources:	

Udacity videos
StackOverflow answers
Official documentation for Flask  - http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/
Official documentation for WSGI - modwsgi.readthedocs.io
https://www.cyberciti.biz/
https://www.tecmint.com/



