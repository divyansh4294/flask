# Flask Overview
## What is Web Framework? 
1. Web Framework is simply collection of library and modules.  
2. It enables a developer to write applications without being bother about low level details such as protocols, thread management etc. 
3. Majority of web frameworks are exclusively by server side technology and some are beginning to include Ajax code which helps in doing task of programming at client side (user browser).  
4. Flask (Discuss in this blog), Django, Pyramid, Turbogears, Web2py (Discuss in later blogs). 

## What is Flask? 

1. It is web framework written in Python.  
1. Developed by Armin Ronacher.  
1. It is refered as micro framework while django is full flash web framework. 
1. Its aims to keep core of application simple yet extinsible.  
1. It doesn't have built-in abstraction layer for database handling nor does it have form validation support. 
1. It supports extension to add such functionality to applications. 

## Setup Flask Framework Environment

### Prerequisite
1. Python 2.6 or higher 

### Install virtualenv (Recommended) 
1. Virtualenv helps is building up python environment.  
2. It helps user to set up separate side by side environment. 
3. Avoid compatibility issues between different version of libraries.  
`<C:\python>pip install virtualenv>`. 
this command need administrator privilages.  
if you are linux/macos user use "sudo" before pip in above command.  
`<$sudo apt pip install virtualenv>`.  
Once installed, new virtual environment created in folder.  
`<mkdir newproj>`  
`<cd newproj>`  
`<virtualenv venv>`  

To activate virtual env  
1. On linux/macos: `<.venv/bin/activate>`  
1. On Windows: `<C:\newproj\>venv\scripts\activate>`  

Now, We are ready to install flask  
`<C:\newproj>pip install flask>`  

## Lets start with Flask  
1. Import flask module  
`<from flask import Flask>`  
1. Flask constructor takes name of current module `<__name__>` as argument.  
1. route() function of flask class is decorator which tells application which URL should call the associated function
`<app.route(rule, options)>`  
* rule: parameter represents URL binding with the function. 
* options: list of parameter forwarded to the rule function. 
1. run() method of flask class used to run the application on local server (localhost). 
`<app.run(host, port, debug, options)>`  
host: Hostname to listen point (Default: 127.0.0.1). 
port: Default is 5000. 
debug: Default is False, You can change if you need. 
options: To be forwarded to underlying server. 

```
from flask import Flask
app = Flask(__name__)
@app.route('/')
def hello_world():
  return "Hello World"
if __name__ == '__main__:
  app.run()
```
Above code script executed using python shell. 
```c:\python>python Hello.py```. 

Now your application is running on:```http://127.0.0.1:5000/```  
or open in browser:```localhost:5000```. 

use ```app.run(debug=True)``` to get enable debug support. 

## Routing in Flask Framework  
1. Modern framework uses routing techniques to help user remembering the applications urls. 
1. Useful to directly access the desired URL without nagivatiing throuht homepage. 
```
from flask import Flask
app = Flask(__name__)
@app.route('/hello')
def hello_world():
  return "Hello World"
if __name__ == '__main__:
  app.run(debug=True)
```
Here, ```@app.route('/hello')``` is bind the the hello_world function. 
User can use function on :```http://127.0.0.1:5000/hello```

## Variable Rules in Flask Framework
1. To build URL's dynamically by adding variable parts to the rule parameter. 
1. This variable marked as ```<Variablename>```. 
1. Passed as Keyword argument to function. 
```
from flask import Flask
app = Flask(__name__)
@app.route('/hello/<name>')
def hello_world(name):
  return "Hello %s!" %name
if __name__ == '__main__:
  app.run(debug=True)
```
Now you can access at ```http://127.0.0.1:5000/hello/divyansh``` and output will be: ```Hello divyansh```. 

## URL Building in Flask Applications

1. url_for() function is very useful for dynamically building url for a specific function  
1. Function accepts name of function as first argument and one or more keywords arguments and each corresponding to variable part of url.  
1. It allows you to change URL in one go, without having to remember to change URL all over the place  
1. If your application is placed outside the URL you are url_for() will handle that properly for you.  
```
from flask import Flask, redirect, url_for
app = Flask(__name__)
@app.route('/admin')
def hello_admin():
  return 'Hello Admin'
@app.route('/guest/<guest>')
def hello_guest(guest):
  return 'Hello %s as guest' % guest
  
@app.route('/user/<name>')
def hello_user(name):
  if name=='admin':
    return redirect(url_for('hello_admin'))
  else:
    return redirect(url_for('hello_guest',guest=name))
if __name__ == '__main__':
  app.run(debug=True)
``` 

## HTTP Methods in Flask Framework

1. HTTP is the protocol to exchange or transfer hypertext  
Some of HTTP Methods are aas follows:  
1. GET: Send data in unencrypted form to server  
1. HEAD: Same as GET, But without response body  
1. POST: Used to send HTML form of data to server
1. PUT:
1. DELETE:
1. TRACE:
1. CONNECT:
