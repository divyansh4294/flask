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

## Request Objects in Flask Framework

1. Data from client web page is sent to server as a global request object.  
1. To process request data, we need to import Flask module.  
1. Some Attributes of request objects are:
  1. Form: A Multidict with parsed form data from POST or PUT requests  
  1. args: A Multidict with parsed content of query string  
  1. values: A combinedMultidict with content of both form and args  
  1. cookies: A dict with the contents of all cookies transmitted with the request  
  1. headers: incoming request headers as dict like object  
  1. data: contains incoming request data as string  
  1. files: A multidict with files uploaded as part of POST or PUT request
  1. method: Current request method (POST, GET etc)
  1. module: Name of the current module if request was dispatched to an actual module.
  1. routing_exception = None: if matching url failed, this is exception will be raised as part of request handling. 
  
Lets create a form and save it as result.html in templates folder:

```
<html>
<body>
  <form action = "http://localhost:5000/result" method='POST'>
  <p>Name <input type="text" name="Name"/></p>
  <p>Physics <input type="text" name="Physics"/></p>
  <p>Chemistry <input type="text" name="Chemistry"/></p>
  <p>Maths <input type="text" name="Mathematics"/></p>
  <p><input type="submit" value="submit"/></p>
  </form>
</body>
</html>
```
Flask code will be as follows: 
```
@app.route('/result',methods=['POST','GET'])
def result():
  if request.method=='POST':
    result = request.form
    return render_template('table.html', result=result)
```
Now table.html contains following code and saved into templates folder  
```
<!doctype html>
<html><body>
<table border=1>
{% for key, value in result.iteritems() %}
  <tr>
    <th>{{key}}</th>
    <td>{{value}}</td>
  </tr>
{% endfor %}
</table>
</body></html>
```
## Cookies in Flask Framework  
1. A cookie's is stored in client's computer as a text file. 
1. Its purpose is to remember and track data pertaining to client's usage for better visitor experience. 
1. Request objects contain cookie attribute which is dictionary object of all cookie variable and corresponding values that client has transmitted.  
1. In flask, cookies are set on response object. 
1. Use make_response() function to get response object from return value if a view function.   
1. Use set_cookie() function of response object to store a cookie ```response.set_cookie(key,value)```.  
1. Use get() function of request.cookies to read the cookie ```request.cookie.get(key)```
```
@app.route('/setcookie',methods=['POST','GET'])
def setcookie():
  if request.method==['POST']:
    user = request.form['nm']
    resp = make_response(render_template('readcookie.html'))
    resp.set_cookie('userID',user)
    return resp
```
```
@app.route('/getcookie')
def getcookie():
  name = request.cookies.get('userID')
  return '<h1>Welcome' +name+'</h1>'
```

Whole program will be as follows:  
```
from flask import Flask, request, render_template, make_response
app = Flask(__name__)

@app.route('/')
def index():
  return render_template("setcookie.html")

@app.route('/setcookie',methods=['POST','GET'])
def setcookie():
  if request.method==['POST']:
    user = request.form['nm']
    resp = make_response(render_template('readcookie.html'))
    resp.set_cookie('userID',user)
    return resp
    
@app.route('/getcookie')
def getcookie():
  name = request.cookies.get('userID')
  return '<h1>Welcome' +name+'</h1>'
  
if __name__=="__main__":
  app.run(debug=True)
```
Create setcookie.html file to take inputs:  
```
<html>
<body>
  <form action = "/setcookie" method='POST'>
  <p><h3>Enter User ID</h3></p>
  <p><input type="text" name="nm"/></p>
  <p><input type="submit" value="Login"/></p>
  </form>
</body>
</html>
```
Create readcookie.html file to take inputs:   
```
<!doctype html>
<html><body>
<a href = 'http://localhost:5000/getcookie'><h2>Click here to read cookies</h2></a>
</body></html>
```

## Session Objects in Flask Framework

1. Unlike cookie, session is stored at server.  
1. Session is time interval between you logged in to logged out of server and data which need to be held across this session is stored in temporary files at server.  
1. Each session has session ID.  
1. Session data is stored on top of cookies and server signs them cyptographically and encryption flask needs SECRET_KEY.  
1. Session object is also dictionary object contains key value pair. 
1. To set secret key: ```app.secret_key='asjajsj/f./f.,/dfgf/,..gf,l;'```
1. To set a session variable: ```session(variable)=value```
1. To release session variable: ```session.pop(variable,none)```
1. Flask will take the value we put into session object and serialise them into a cookie.

```
from flask import Flask, request, url_for, redirect, render_template, session
app = Flask(__name__)
app.secret_key="any random string"

@app.route("/")
def index():
  if 'username' in session:
    username = session['username']
    return 'Logged in as'+username+"<br>"+\
           "<b><a href = "/logout">Click here to logout</a></b>"
  return "You are not logged in <br><a href = "/login"></b>"+ \
          "Click here to login </b></a>"
           
@app.route("/login", methods=['POST','GET'])
def login():
  if request.method==['POST']:
    username = request.form['username']
    return redirect(url_for('index'))
  return render_template("session.html")
    
@app.route("/logout")
def logout():
  session.pop('username',None)
  return redirect(url_for('index'))
  
  
if __name__=="__main__":
  app.run(debug=True)
```
Create a session.html file:
``` 
<!doctype html>
<html><body>
<form action = "" method = "POST">
    <p>User Name:<input type = "text" name = "username"/></p>
    <p><input type = "Submit" name = "login"/></p>
</form>
</body></html>
```

## Redirects and Errors in Flask Framework

1. Flask class has a redirect() function which return a response object and redirect user to target location with specified status code.  
1. To redirect: ```Flask.redirct(location, statuscode, response)```
1. Standarised Status code as below:
  1. HTTP_300_MULTIPLE_CHOICES
  2. HTTP_301_MOVED_PERMANENTLY
  3. HTTP_302_FOUND(Default)
  4. HTTP_303_SEE_OTHER
  5. HTTP_304_NOT_MODIFIED
  6. HTTP_305_USE_PROXY
  7. HTTP_306_RESERVED
  8. HTTP_307_TEMPORARY_REDIRECT

```
from flask import Flask, request, url_for, redirect, render_template
app = Flask(__name__)
app.secret_key="any random string"

@app.route("/")
def index():
  return render_template('login.html')
           
@app.route("/login", methods=['POST','GET'])
def login():
  if request.method==['POST'] and request.form['username'] == 'admin': 
    return redirect(url_for('success'))
  return redirect(url_for('index'))
    
@app.route("/success")
def success():
  return "Logged in successfully"
  
  
if __name__=="__main__":
  app.run(debug=True)
```

1. Flask has a early exit funtion with a error code abort()
2. ```Flask.abort(code)```
3. Code parameters are as follows:  
  1.400 for Bad Request
  1. 401 for Unauthenticated
  1. 403 for Forbidden
  1. 404 for Not Found
  1. 406 for Not Acceptable
  1. 415 for Unsupported Media Type
  1. 429 Too Many Requests

```
from flask import Flask, request, url_for, redirect, render_template, abort
app = Flask(__name__)
app.secret_key="any random string"

@app.route("/")
def index():
  return render_template('login.html')
           
@app.route("/login", methods=['POST','GET'])
def login():
  if request.method==['POST']:
    if request.form['username'] == 'admin':
      return redirect(url_for('success'))
    else:
      abort(401)
  else:
    return redirect(url_for('index'))
    
@app.route("/success")
def success():
  return "Logged in successfully"
  
  
if __name__=="__main__":
  app.run(debug=True)
  
```

## Message Flashing in Flask Framework

1. Desktop applications use dialog messages.
2. It create a message in one view and render it in a view function called next
3. Flask.flash() method passes a message to next request which is generally is a template ```Flask.flash(message, category(optional))```
4. In order to remove messages from sessions, template calls ```get_flashed_messages(with_categories, category_filter)```
5. with_categories: It is a tuple if received messages having category.
6. category_filter: It is used when we have to display specific message.

```
from flask import Flask, request, url_for, redirect, render_template, abort, flash
app = Flask(__name__)
app.secret_key="any random string"

@app.route("/")
def index():
  return render_template('index.html')
           
@app.route("/login", methods=['POST','GET'])
def login():
  error = None
  if request.method == 'POST':
    if request.form['username'] != 'admin' or if request.form['password'] != 'admin':
      error='Invalid Username or password Please try again'
    else:
      flash('You are successfully logged in')
      flash('Log out before login again')
      return redirect(url_for('index'))
  return render_template('log_in.html',error=error)
if __name__ == "__main__":
  app.run(debug=True)
```
Create a index.html file: 
``` 
<!doctype html>
<html><body>
<h1>Login</h1>
{% if error %}
<p><strong>Error:</strong> {{error}}

<title>Flask Flashing Massages</title>
</head>
<body>
<h1>Message</h1>
{% with messages = get_flashed_messages() %}
{% if messages %}
<ul>
{% for message in messages %}
<li>{{ message }}</li>
{% endfor %}
</ul>
{% endif %}
{% endwith %}
<p>Do you want to <a href = "{{ url_for('login')}}"><br>log in?</b></a>
</body></html>
```
Create login.html file:
```
<!doctype html>
<html><body>
<h1>Login</h1>
{% if error %}
<p><strong>Error:</strong> {{ error }}
{% endif %}
<form action="" method=['POST']>
<dl>
<dt>Username:</dt>
<dd>
<input type = 'text' name ='username' value="{{request.form.username}}">
</dd>
<dt>password:</dt>
<dd>
<input type = 'password' name ='password'>
</dd>
</dl>
<p><input type = "submit" name ="Login"></p>
</form>
</body>
</html>
```

## File Uploading in Flask Framework

1. Some prerequisite is HTML form with its enctype attribute set to "multipart/form-data" and method attribute is set to POST.  
1. URL handler fetches file from ```request.files[]``` object and save to location.
1. first every file is saved to temporary location on server before actually saved to its ultimate location. 
1. Destination can be hard coded or can be obtained from filename property of ``` request.files[file]``` object. 
1. To get secure version of file name use ```from werkzeug import secure_filename``` 
1. Define path of default upload file ``` app.config("UPLOAD_FOLDER")``` and maximum size of file (BYTES) can be configured ``` app.config("MAX_CONTENT_PATH")```
```
from flask import Flask, request, render_template
from werkzeug import secure_filename
app = Flask(__name__)

@app.route("/upload")
def upload():
  return render_template('upload.html')

@app.route("/uploader", methods=['POST','GET'])
def uploader():
  if request.method=='POST':
    f = request.files['file']
    f.save(secure_filename(f.filename))
    return "File uploaded successfully"
  
if __name__=="__main__":
  app.run(debug=True)
```
Create upload.html file:
```
<html>
  <body>
    <form action="http://localhost:5000/uploader" method='POST' enctype = "multipart/form-data">
      <input type = "file" name='file' />
      <input type = "submit" />
    </form>
  </body>
</html>
```

## Extensions in Flask Framework

Some of the flask extensions are as follows:
1. Flask mail: Provides SMTP interface to flask application
2. Flask WTF: adds rendering and validation of WTForms
3. Flask SQLalchemy: adds dqlalchemy support to flask application
4. Flask Sijax: Makes ajax easy to use in web applications

