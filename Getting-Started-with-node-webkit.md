This chapter is contains some tutorial information to get you started with node-webkit programming. It assumes that you have node-webkit binaries. If you want to build node-webkit itself first, refer to the [[Building node-webkit]] section.

## Basics

To begin our introduction to node-webkit, we'll start with the simplest program possible.

**Example 1. Hello World**

![Capture3](https://f.cloud.github.com/assets/2891424/279516/5fba0cca-912b-11e2-983d-c2e8a66c3706.PNG)

Create `index.html`:

````html
<html>
<head>
<title>Hello World!</title>
</head>
<body>
<h1>Hello World!</h1>
</body>
</html>
````

Create `package.json`:

````json
{
  "name": "nw-demo",
  "main": "index.html"
}
````

Compress `index.html` and `package.json` into a zip archive, and rename
it to `app.nw`:

    app.nw
    |-- package.json
    `-- index.html

Download the prebuilt binary for your platform and use it to open the
`app.nw` file:

````bash
$ ./nw app.nw
````

Note: on Windows, you can drag the `app.nw` to `nw.exe` to open it.
