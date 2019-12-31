# NODE
--

##### Upgrading

```
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```

##### Environments

`export NODE_ENV=production` setting machine environment

`NODE_ENV=production node xxxx.js` setting and starting on same line



##### Middleware

`basicAuth` basic access authorization. Keep in mind that basic auth offers only the most basic security, and you should use basic auth only over HTTPS (otherwise, usernames and passwords are transmitted in the clear). You should use basic auth only when you need something very quick and easy and you’re using HTTPS.

`body-parser` simply links in json and urlencoded.

`json` Parses JSON-encoded request bodies.

`urlencoded` (see body-parser) for ajax `application/x-www-form-urlencoded` body.

`Formidable` for multipart form processing.

`compress` Compresses response data with gzip.

`cookie-parser` cookie support.

`cookie-session` cookie-storage session support. not recommended

`express-session` session ID (stored in a cookie) session support

`csurf` protection against cross-site request forgery (CSRF) attacks.

`directory` directory listing support for static files.

`errorhandler` stack traces and error messages to the client.

`static-favicon` serves the “favicon” from root of static folder favicon.ico.

`morgan` automated logging support: all requests will be logged.

`method-override` support for the x-http-method-override request header, which allows browsers to “fake” using HTTP methods other than GET and POST. This can be useful for debugging. Only needed if you’re writing APIs.

`query` Parses the querystring and makes it available as the query property on the request object. This middleware is linked in implicitly by Express, so do not link it in yourself.

`response-time` Adds the X-Response-Time header to the response, providing the response time in milliseconds.

`static` support for serving static (public) files. You can link this middleware in multiple times, specifying different directories.

`vhost` Virtual hosts (vhosts), a term borrowed from Apache, makes subdomains easier to manage in Express.

`nodemailer` email sending


##### create a new npm repository project

command | description
--------|------------
`npm init`                     | creates a package.json
`npm install <package>`        | install package for dev/testing only
`npm install <package> --save` | install package (inc. package.json dependency)
`npm start`                    | start the default process
`npm install express@latest --save --force` | re-install latest version
 

### Testing with mocha

command | description
--------|------------
`npm install --save-dev mocha` | Installing mocha test for dev only in project
`npm install -g mocha`         | Installing mocha test framework globally

Testing cross-page (needs node server running and zombie browser)

`mocha -u tdd -R spec qa/tests-crosspage.js 2>/dev/null`

Testing logic with unit tests

`mocha -u tdd -R spec public/qa/tests-unit.js`


### Tooling

command | description
--------|------------
`npm install -g jshint`        | installing jshint globally
`jshint meadowlark.js`         | running jshint on application code for hints
<http://wummel.github.io/linkchecker/> | Install and point to website for link checking


`sudo npm install -g grunt-cli`

`npm install --save-dev grunt`

`npm install -g node-inspector`

`node-debug app.js` then run chrome on port reported for debugger


#### node-debug

`npm install -g node-inspector` install inspector

`node-dev app.js` start app in debug mode


#### forever
forever is not only a great CLI tool for keeping your app running continuously in a production environment, but also has advanced features that support running multiple node processes as background services. In this case, it gives you the option of restarting the server on file changes by using the -w flag. By using this flag, we not are only automatically restarting the application when the code changes, but also when and if the application goes down.

command | description
--------|------------
`sudo npm install -g forever` | Installation
`forever -w app.js`           | Use forever -w, instead of node to start your app

#### nodemon
I found out about Remy Sharp’s nodemon while browsing StackOverflow and have been using it for developing Node.js apps ever since. It’s fast and very simple to use… and it also supports CoffeeScript:

command | description
--------|------------
`sudo npm install -g nodemon` | Installation
`nodemon app.js`              | Use nodemon, instead of node
`nodemon -e ".coffee|.js|.ejs" app.js` | Specify custom extensions to watch


### Databases

command | description
--------|------------
`npm install --save mongoose` | MongoDB node driver
`npm install --save session-mongoose` | MongoDB as Session Storage (didn't work)
`npm install --save mongoose-session` | MongoDB as Session Storage

### React

`jsx --watch src/ build/` for pre-compiling the react source into target folder

