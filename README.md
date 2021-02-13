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
1. PUT: Replace all current representation of target resources with uploaded content.  
1. DELETE: Removes all current representation of target resource given by URL  
1. TRACE: Echoes received request so that a client can see changes(if any)   
1. CONNECT: COnvert request connection to a transparent TCP/IP tunnel to facilitate SSL-encrypted communication.  

By default, Flask route responds to GET Requests.  

```
from flask import Flask, request
app = Flask(__name__)
@app.route('/function', methods=['POST','GET'])
def function():
  if request.method == "POST":
    #Statement1
  else:
    #Statement2
``` 

To support this, We need to create a html form so that we can take input from their.  

```
<html>
<body>
  <form action="http://localhost:5000/login" method="post">
  <p>Enter Name:</p>
  <p><input type="text" name="nm" /></p>
  <p><input type="submit" value="submit /></p>
  </form>
</body>
</html>
```
So the flask code will be as follows:  
```
from flask import Flask, request, redirect, url_for
app = Flask(__name__)
@app.route('/welcome/<name>')
def welcome(name):
  return 'welcome %s' %name
  
@app.route('/login', methods=['POST','GET'])
def login():
  if request.method == "POST":
    user=request.form('nm')
    return redirect(url_for('welcome',name=user))
  else:
    user=request.args.get("nm")
    return redirect(url_for('welcome',name=user))
 
 if __name__ == '__main__':
  app.run(debug=True)
``` 

## Templates in Flask Framework

* Create a folder named 'templates' where all python files saved.  
* All html files would be stored in templates and that are rendered using flask.  

```
<html>
<body>
<h1> Hello World!</h1>
</body>
</html>
```
Save it as ```hello.html``` in templates folder  

```
from flask import Flask, render_template
app = Flask(__name__)
@app.route('/')
def index():
  return render_template('hello.html')
 
if __name__ == '__main__':
  app.run(debug=True)
``` 
Now visit ```http://localhost:5000``` to check output but this time is comes from hello.html  

## Static Files in Flask Framework

1. Web app requires static files  
1. JS files or CSS file supporting web pages.  
1. All this static files shpuld be kept in Static folder  
1. Available at /static on the application  
1. endpoint 'static' used to generate url for static files: ```url_for('static',filename='style.css')```  
1. app.static_url_path: can be used to specify different path for static files on web  
1. app.static_folder: folder with static files that should be served at static_url_path.  

Lets create a html file named index.html and save it into templates folder:
```
<html>
<head>
<script type = "text/javascript" src = "{{ url_for('static',filename='hello.js')}}" ></script>
</head>
<body>
<input type = "button" onclick = "sayHello()" value = "Say Hello" />
</body>
</html>
```
Lets create a JavaScript file named hello.js and save it intp static folder:  
```
function sayHello() {
alert("Hello World")
}
```
Now, the flask code will be as follows:  
```
from flask import Flask, render_template
app = Flask(__name__)
@app.route('/')
def index():
  return render_template('index.html')
 
if __name__ == '__main__':
  app.run(debug=True)
``` 
Visit ```http://localhiost:5000``` and click on Say Hello button to see the magic.  


