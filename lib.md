## Angular Library
<p>
Creating angular library
</p>


#### Creating workspace, library, application
````
ng new workspace --create-application=false
ng g library my-lib --prefix=njx
ng new myapp

package.json
"lib:build"  : "ng build my-lib"
"lib:package": "cd dist/my-lib && ng pack"
"lib:publish": "cd dist/my-lib && ng pack && npm publish"


Building
npm run lib:build
npm run lib:package

Running
ng serve my-lib  --watch
ng serve myapp


Create npm package
register user in www.npmjs.com
npm adduser

Publish 
npm run lib:publish

````

