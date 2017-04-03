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

