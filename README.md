![PGS Software](screenshots/pgssoft-madewithlove.png)

# Uglyfier for Android
##### Gradle script for downsizing graphical assets in development builds

### Problem
Usually you don't care how big the deployed APK is. However, if you are using metered connection and remote ADB, it would be good idea to build possibly smallest APK file.

### Solution
Uglyfier for Android is a gradle script which will shring all image resources in your application during build. It will look ugly, but image files will be 10 times smaller. **Ninepatch (9-patch) PNGs are fully supported, with transparency preserved!**

### Installation

1. Install [ImageMagick](https://www.imagemagick.org)
2. Download [uglyfy.gradle](https://raw.githubusercontent.com/tomekziel/uglyfier/master/uglyfy.gradle) and save it to application directory (where `build.gradle` live)
3. Create new buildType, i.e. "uglyfied". You can define it to inherit existing one:
    ```groovy
    buildTypes {
        debug {
            debuggable true
        }

        uglyfied.initWith(buildTypes.debug)
        uglyfied{}
    ```
4. Add following snippet to your `build.gradle`, adjusting all three variables.
    ```groovy
    
    ext {
        UglyfierSourceVariant      = 'debug'     // this is the source build type
        UglyfierDestinationVariant = 'uglyfied'  // this is the target, uglyfied build type

        // provide path to Imagemagick here
        // (or remove this line and set IMAGEMAGICK_EXECUTABLE environment variable
        UglyfierImagemagickPath = 'c:/Program Files/ImageMagick-7.0.1-10-portable-Q16-x86/magick.exe' //sample value for Windows
        //UglyfierImagemagickPath = '/usr/bin/convert' //sample value for Linux
    }
    apply from: 'uglyfy.gradle'    
    ```
5. Try to build new "uglyfied" variant.


### What will happen?
During build following events will take place:

1. Image files (PNG, JPG, JPEG) from "main" resource directory will be copied to "uglyfied" resource directory, then downsized.
2. Image files from source variant (i.e. "debug") will be also copied to "uglyfied" resource directory, then downsized.
3. Any extra resource files from source variant (i.e. "debug") will be also copied to "uglyfied" resource directory.

### How the downsizing works?

JPEG files are heavily recompressed, with target quality=5. Compression artifacts are expected.

PNG files are downsized to 10% of size, then scaled back. Big pixel blocks are expected. **Ninepatch (9-patch) files are fully supported, 1-pixel frame is preserved! (as well as transparency)**

Files smaller than 5KB are skipped.

### Results
We tested Uglyfier with our internal projects that contain many graphical assets and here are our results: 

<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>Normal image resources size</th>
            <th>Uglyfied image resources size</th>
            <th>Size reduction</th>
        </tr>
    </thead>    
    <tr>
        <td align="center">18124 KB</td>
        <td align="center">3060 KB</td>
        <td align="center">83%</td>
    </tr>
    <tr>
        <td align="center">9258 KB</td>
        <td align="center">861 KB</td>
        <td align="center">91%</td>
    </tr>
</table>

Below you can see what is the difference in application's assets quality with uglyfied and standard build:

<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>Standard (top) / Uglyfied (bottom)</th>
        </tr>
    </thead>    
    <tr>
        <td align="center"><img src="/screenshots/mixed1.png" /></td>
    </tr>
    <tr>
        <td align="center"><img src="/screenshots/mixed2.png" /></td>
    </tr>
    <tr>
        <td align="center"><img src="/screenshots/mixed3.png" /></td>
    </tr>
    <tr>
        <td align="center"><img src="/screenshots/mixed4.png" /></td>
    </tr>
</table>

### TODO

 * [ ] Better build event injection than `clean.doLast`
 * [ ] Re-uglyfying based on file modification time, not every time

### Contributing

Bug reports and pull requests are welcome on GitHub at [https://github.com/PGSSoft/uglyfier](https://github.com/PGSSoft/uglyfier).

### License

The project is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

### About

The project maintained by [software development agency](https://www.pgs-soft.com/) [PGS Software](https://www.pgs-soft.com/).
See our other [open-source projects](https://github.com/PGSSoft) or [contact us](https://www.pgs-soft.com/contact-us/) to develop your product.

### Follow us

[![Twitter URL](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=https://github.com/PGSSoft/uglyfier)  
[![Twitter Follow](https://img.shields.io/twitter/follow/pgssoftware.svg?style=social&label=Follow)](https://twitter.com/pgssoftware)
