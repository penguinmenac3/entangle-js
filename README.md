# Entangle-JS [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Did you find syncing your javascript variables with html elements annoying.
Or did you ever think syncing variables between two processes (anywhere in the world) is difficult?
Entangle-JS is your solution.
It enables you to entangle two variables across processes and to a special html entanglement tag.
One needs to launch an entanglement server and one an entanglement client.
You can connect entanglements even cross programming language.

You can see the documentation below or simply look at some examples.

A complete website with an entanglement can be found [here](https://github.com/penguinmenac3/entangle-js/blob/master/example.html).
If you use remote entanglement you will need an server for example written in [entangle-python](https://github.com/penguinmenac3/entangle-python).
Check out [this script](https://github.com/penguinmenac3/entangle-python/blob/master/example_server.py) if you want to write a python entanglement server.


## Setup

Simply copy the [entangle.js](https://github.com/penguinmenac3/entangle-js/blob/master/entangle.js) file into your project and add a script tag as shown in the usage section.

## Usage

### HTML

In your header include the following two lines.

```html
<script src="entangle.js"></script>
<script type="text/javascript">remote_entanglement(host, port, passwd, js_callback_or_null)</script>
```

In the body of your code whenever you want to use a variable that is entangled, simply add the entanglement attribute with the name of the entanglement.
An example:

```html
<div entanglement="test">Entangling in process..</div>
```

Now you just need a server which hosts the entanglement. And sets all variables you use.
Whenever the server changes a variable it changes in (more or less) realtime in the webpage.

The simplest way of writing an entanglement server is by using entangle-python.

### Javascript

Furthermore you can also call remote functions and set variables for the server to read with javascript.
And of course javascript has also access to all entangled variables.

```javascript
function callback(entanglement) {
  entanglement.set("test", "My value");
  entanglement.remote_fun("my_fun", 1, 2, "hello"); // -> my_fun(1, 2, "hello")
  var test = entanglement.get("test");
}
```

### Local Only Entanglement

You can also use entanglement to entangle your javascript with your html without a server.
Use local_entanglement for that and your entanglement function should now only use entanglement.set function and the get function.

```html
<html>

<header>
  <script src="entangle.js"></script>
  <script type="text/javascript">
    function callback (entanglement) {
      entanglement.set("test", "Hello from JS!");
    }
    local_entanglement(callback)
  </script>
</header>

<body>
  <div entanglement="test">Hello HTML!</div>
</body>

</html>
```

## API

### local_entanglement(callback)

Do a local entanglement between javascript and html.

callback: function(entanglement)

### remote_entanglement(url, port, passwd, callback)

Create a remote entanglement between this html and js and a remote server.

url: string
port: int
passwd: string
callback: function(entanglement)

### entanglement.set(name, value)

Set the value of a variable in the entanglement.
Updates the value in every entanglement.

### entanglement.get(name) -> value

Read the current value of an entanglement.

### entanglement.remote_fun(name) -> function(...args)

Get a handle to a remote function of the given name.
You must provide the correct arguments in the correct order as the remote expects them.

Only works with remote_entanglement.

### entanglement.remote_fun_kwargs(name) -> function({key: value}, ...args)

Get a handle to a remote function of the given name.
You must provide the correct arguments in the correct order as the remote expects them.

kwargs is a dictionary containing named arguments.
As some languages such as python might want named arguments for some functions.

Only works with remote_entanglement.
