iRequire
==========

How awesome would it be if you could require your favorite javascript piece of code with relevant css and html just by doing something like:

```sh
require("bootstrap")
//or
require("jquery-ui")
```

iRequire is a tiny (5k unminified) script that allows you to do so with **zero or few configuration** and **automatic dependency injection**.

Dependency
==========
iRequire is dependent on [jQuery] framework which is automatically loaded!

How it works
==========
iRequire expects a folder structure like:

```sh
+-- modules
|   +-- irequire
|   |   +-- irequire.js
|   |   +-- jquery.js
|   |   +-- package.json
|   +-- mymodule
|   |   +-- mymodule.css
|   |   +-- mymodule.html
|   |   +-- mymodule.js
|   |   +-- package.json
|   +-- myothermodule
|       +-- myothermodule.js
|       +-- package.json
+-- index.html
```

After including it with a script tag in your index.html:
```sh
<script type="text/javascript" src="./modules/irequire/irequire.js"></script>
```

Imagine the package.json under "mymodule" folder would look like:
```sh
{
  "name": "mymodule",
  "version": "0.0.1",
  "main": [
    "mymodule.css",
    "mymodule.html",
    "mymodule.js"
  ]
}
```
You could now require your module:
```sh
require("mymodule")
```

And the three files declared in the main array would be loaded and:
  - mymodule.css would be included as the href of a &lt;link&gt; tag appended to the &lt;head&gt; of the document.
  - mymodule.html would be appended to the &lt;body&gt; of the document.
  - mymodule.js would be evaluated.

iRequire is event oriented so, after the loading of the above files, it will trigger a "mymoduleready" event to which you may respond accordingly by doing something like:
```sh
$("body").on("mymoduleready",function(){
    //Do something wonderful with it!
});
```

Now imagine that "myothermodule" is dependent on "mymodule" and its package.json file looks like:

```sh
{
  "name": "myothermodule",
  "version": "0.0.1",
  "main": [
    "myothermodule.js"
  ],
  "dependencies" : {
    "mymodule" : "0.0.1"
  }
}
```

If you tried to require "myothermodule" before requiring "mymodule", both would be required in the correct order (dependencies first) and there would be triggered a ready event for each of them.

In the previous example, as "mymodule" would be already loaded, requiring "myothermodule" would only load the second.

License
----

MIT

[jQuery]:http://jquery.com
