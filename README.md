# Uglyfier for Android
##### Gradle script for downsizing graphical assets in development builds

### Problem
Normally, you wouldn't care how much data is consumed to transfer APK file when deploying it using remote ADB connection, however, if you use your personal mobile hotspot, it would be good to build possibly smallest APK to prevent all your mobile data from being consumed after just few hours of work.

### Solution
If, during development, small APK size matters more than high quality graphical assets, then it would be convenient to have a build variant that automatically shrinks all images found in project when generating an APK. That's exactly what Uglyfier does.

### How does it work?
Uglyfier is a gradle script that can be applied during build process. It uses specified build variant directory for its output, so the original files remain untouched. It scans whole project looking for image files (jpg and png) and depending on file type - reduces its quality (for jpg images) or used number of colors (for png) using [ImageMagick](http://www.imagemagick.org/). After saving all the images in a seperate build variant directory, build task proceeds and the downsized APK is generated.

Uglyfier automatically merges all files necessary to build project. You can specifiy source build variant to include its files during merge. For example, for following configuration:

```groovy
ext {
    destinationVariant = 'uglyfied'
    sourceVariant = 'signedDebug'
}
```
Uglyfier will merge following files:
<img style="display: block" src="/screenshots/filetree.jpg"/>

### Results
We tested Uglyfier with our internal project that contain many graphical assets and here are our results: 

<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>Normal APK size</th>
            <th>Uglyfied APK size</th>
            <th>Size reduction</th>
        </tr>
    </thead>    
    <tr>
        <td align="center">22,688 KB</td>
        <td align="center">5,486 KB</td>
        <td align="center">~76 %</td>
    </tr>
    <tr>
        <td align="center">14,021 KB</td>
        <td align="center">7,850 KB</td>
        <td align="center">~44 %</td>
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
        <td align="center"><img src="/screenshots/normal1.png" /><img src="/screenshots/uglyfied1.png" /></td>
    </tr>
    <tr>
        <td align="center"><img src="/screenshots/normal2.png" /><img src="/screenshots/uglyfied2.png" /></td>
    </tr>
</table>

### Prerequisites
* install [ImageMagick](http://www.imagemagick.org/),
* take a look at sample [build.gradle](sample/build.gradle) file.