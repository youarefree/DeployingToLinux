# This is a README file giving details about the project deploying the Item Catalog project to a Linux Server:

IP:18.197.102.169
SSH port: 2200
URL: http://18.197.102.169/ # Still haven't given it a domain name but will do

After setting up the server in Amazon Lightsail I had to take care of the ports and the users, thus creating a controlled environment.
Having done this, I started working on setting up a web server which in this case is a Apache2 server. After installing it, I had to 
configure its URLs and ports as well and run it. 

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

	
