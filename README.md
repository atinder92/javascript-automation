# javascript-automation
### Implementing Automation using GULP.js
GULP.js is very popular javascript based library used for automating common tasks such as minification files, concatenating files, testing, quality checking, etc. With GULP.js, you write tasks in the form of code withing gulp.js files and then you simply run these files. 

### API in GULP.js

GULP.js has common API's for writing your automating code:
1. gulp.task 
2. gulp.src
3. gulp.dest
4. gulp.watch

#### Very basic example
```
gulp.task("taskname",function(){

return gulp
.src('./src/**/*.js')
.pipe(concat('all.js'))
.pipe(uglify())
.pipe(gulp.dest('./build/'));

})


```

### Installing GULP.js and Bower on MAC
Use the following command to install GULP.js and Bower on your MAC

``` 
npm install -g gulp 
npm install -g bower 
```

To verify whether gulp and bower has been installed, just use the following commands
```
gulp -v
bower -v 
```

To check what packages we have installed using npm, use the following command
```
npm list -g --depth=0
```

### Using JSHINT, JSCS and other useful plugins in GULP.js
JSHINT and JSCS are both used for checking errors in the javascript code and also for quality check.
JSHINT checks whether your code is error free or not, whereas JSCS is useful for checking your code against team style guideline.

Installing jshint and jscs using the following commands:
```
npm install jshint gulp-jshint --save-dev
npm install jscs gulp-jscs --save-dev

```
To include this into your project, use the following code in your javascript file
```
var jshint = require('gulp-jshint');
var jscs = require('gulp-jscs');
```

The following example shows a simple example of using jshint and jscs in your code:
```
gulp.task('checkmycode',function(){
    
return gulp
        .src([
        './my-source-code/**/*.js',
        './*.js'
        ])
        .pipe(jscs())
        .pipe(jshint())
        .pipe(jshint.reporter('jshint-stylish',{verbose:true}))
    
});
```

The example above also use jshint-stylish plugin (which simply shows errors in more verbose way) and you can install jshint-stylish using the following command:
```
npm install jshint-stylish --save-dev

```
When ready, go to command line and enter the following command:

`gulp checkmycode`

Now Let's say you wanna display some message on console, you can use log function from gulp-util plugin

Install gulp-util using the command

`npm install gulp-util --save-dev`

Include it in your gulpfile.js 

`var util = require('gulp-util')` 

The example below shows the working example for the gulp-util:

```
gulp.task('checkmycode',function(){

log("### Checking javascript code ###");    
    
return gulp
        .src([
        './my-source-code/**/*.js',
        './*.js'
        ])
        .pipe(jshint())
        .pipe(jshint.reporter('jshint-stylish',{verbose:true}));
        
        
    
});

function log(msg){

    util.log(util.colors.blue(msg));
}


```

Note: You can use util.colors to display the message on console in different colors.

***

### CSS Compilation

#### Using GULP-LESS

Gulp-less is used for compiling less files into css files.

Use the following command to install gulp-less plugin:

`npm install gulp-less --save-dev` 

To include the plugin into your gulpfile.js, use the following code:

`var gulpless = require('gulp-less');`


#### Using GULP-AUTOPREFIXER
Gulp-autoprefixer is very useful for adding the prefixes to your css code, so that it could be run in different browsers.

Use the following command to install gulp-autoprefixer plugin:

`npm install gulp-autoprefixer --save-dev`

To include the plugin into your gulpfile.js, use the following code:

`var gulpprefixer = require('gulp-autoprefixer');`


#### Example of using gulp-less and gulp-autoprefixer

The following example get a less file from a folder, saves it to destination and also add prefixes for different browsers:

```
gulp.task('mycsstask',function(){

    
    log("### Doing CSS compilation ###");
    return gulp.src('./source/mylessfile.less')
    .pipe(gulpless())
    .pipe(gulpprefixer())
    .pipe(gulp.dest('./destination/'));


});

```
Run the above gulp task using the following command:

`gulp mycsstask`

The above code will compile the less file into css file and also adds vendor prefixes.

#### Using watch API of Gulp.js

Watch API for gulp.js is very useful for checking changes in your code and performing your gulp task, when a change is made.

Example:

```
gulp.task('watch-my-less-files',function(){


gulp.watch(['./my-source-code/less/myless.less'],['mycsstask']);


});

```

When you run the above task, gulp will start watching for changes in your code. If any change occur in your code, then gulp will run __mycsstask__


***

### Adding javascript and css files references to HTML files automatically

One of the useful package which we can use to automatically add javascript and css references from bower configuration file  to our html files is __wiredep__ 

Install wiredep using the following command :

`npm install wiredep --save-dev`

You can include this package into your gulpfile.js as follow:

`var wiredep = require('wiredep').stream;`


Example :

```
// name of gulp task is go-add-bower-references

gulp.task('go-add-bower-references',function(){

//basic configuration for wiredep

var wiredep_config = {
directory:'./bower_components/',
bowerJson:require('./bower.json')
};

return gulp.src('./my-source-code/index.html')// your index.html file 
.pipe(wiredep(wiredep_config)) // 
.pipe(gulp.dest(./my-source-code/dest/));// save it to destination

});

```

Before running the command, remove all your hard coded bower references from your html file and add the following code where you want to add your css and javascript references:

```
<!-- bower:css -->

<!-- endbower -->


<!-- bower:js -->

<!-- endbower -->
```


Now Run the above task using following command:

`gulp go-add-bower-references`

You will see that wiredep automatically adds your all css and javascript references to your html file.

### BrowserSync

BrowserSync has built a cool npm package, which we can use into our projects. It enable us to load the browser instantly if any change occur in our code. It watches our files and if any change happen in our file, it loads the browser. 

Install Browser Sync using following command:

`npm install browser-sync --save-dev`

Include browser-sync into your code as follows:

`var browserSync = require('browser-sync').create();`

Below is the very simple example, which reloads the html file whenever the css file change:

```
gulp.task('run-browser-sync',function(){

  
    var browserSyncConfig = {
    
        proxy:"localhost:8080/index.html",
        files:['./mycssfile.css'],
        notify:true,// a small pop-over notifications in the browser
        reloadDelay:0 
    
    };
    

    browserSync.init(browserSyncConfig);
    
    
});


```

Run the above task using following command:

`gulp run-browser-sync`


***

### GULP USEREF
GULP useref is a great plugin, if you want to concatenate your files. For example, you can concatenate your 10's of javascript files into just one file.

Use the following command to install Gulp-useref:

`npm install gulp-useref --save-dev`

Include gulp-useref into your javascript code as follows:

`var useref = require('gulp-useref');`

A very basic example has been shown below:
```
gulp.task('combine-my-files',function(){

    log("### Combining your files ###");
    
    return gulp.src('./index.html')
           .pipe(useref()) // process the index.html, combines files here
           .pipe(gulp.dest('./build/')); // put the index.html into build folder


});
```
Run the above task using the following command:

`gulp combine-my-files`

The sample code in the index.html is shown below:
```
<!-- build:css ./css/combinedcss.css -->    
<link rel="stylesheet" type="text/css" href="my-source-code/one.css">   
<link rel="stylesheet" type="text/css" href="my-source-code/two.css">   
<!-- endbuild -->

<!-- build:js ./scripts/combinedjs.js -->    
    
<script src="my-source-code/one.js" type="text/javascript"></script>    
<script src="my-source-code/two.js" type="text/javascript"></script>    
<!-- endbuild -->

```

The gulp-useref will combine files which you mentioned in the build tag. You have to specify type of files inside the build tag(example: js or css) and also the location, where you will save your combined file.



});












