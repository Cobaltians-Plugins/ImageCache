# AssetCache Plugin

AssetCache plugin allows you to download a file from the world wide web and download it to the local web folder of your app. 

Then, you will be able to access it from web source or delete it aswell.

## How to use

* import the plugin to your project as explained [here](https://github.com/cobaltians/cobalt/wiki/Plugins-usage)
* Add the cobalt.assetcache.js to your web JS folder
* Add an html link to the cobalt.assetcache.js plugin script after the cobalt link in the HEAD tag

## download

Downloads a distant file and store it locally in the web folder.

Use the `cobalt.assetCache.download` shortcut like this

    cobalt.assetCache.download({ 
        path: 'download/subfolder/image.png', 
        url : 'https://kristal.io/img/logo-kristal-fr.9bab385.png'}, 
        function(data){
          cobalt.log('status', data.status, data);
    });

Here is the detail of `data` sent by the web : 

* `path` : (string) The destination local path of the file (subfolder/file.png)
* `url` : (string) The url of the file on the internet


Notes : 
- Path is relative to app web folder defined in cobalt. (assets or assets/www for example on Android).
- Path is also used as an id for status events or delete method.

### callback

Native side is sending download progress events using the web callback. Here is the data that can be sent in this callback :

**Download progress**

`{ path : 'yourDestinationFilePath', status: 'downloading', progress : 42 }`

File with detination path `yourDestinationFilePath` is downloading. Download progress is at 42%.

**Download success and file available**

`{ path : 'yourDestinationFilePath', status: 'success', root: 'file://.../' }`

File with detination path `yourDestinationFilePath` has been successfully downloaded and is availbale for web use.

* `root` is the local file folder path. You should concatene root+path to access the file in the webview.

**Errors**

Network errors :

`{ path : 'yourDestinationFilePath', status: 'error', cause : 'networkError' }`

Got a 404 error from distant server :

`{ path : 'yourDestinationFilePath', status: 'error', cause : 'fileNotFound' }`

Error writing local file : 

`{ path : 'yourDestinationFilePath', status: 'error', cause : 'writeError' }`

All other errors

`{ path : 'yourDestinationFilePath', status: 'error', cause : 'unknownError' }`

## delete

Deletes a local file.

Use the `cobalt.assetCache.delete` shortcut like this

    cobalt.assetCache.delete({ 
        path: 'download/subfolder/image.png', 
        function(data){
          cobalt.log('status', data.status, data);
    });

Here, web is only sending path

* `path` : (string) the local path of the file to delete.

As for download, path is relative to web assets folder defined in Cobalt.

### callback

Native side is sending callback to keep web updated of the deletion. Here are the possibilities :

**Success (file deleted)**

`{ path : 'yourDestinationFilePath', status: 'success' }`

**Errors**

File not found

`{ path : 'yourDestinationFilePath', status: 'error', cause : 'fileNotFound' }`

Other errrors

`{ path : 'yourDestinationFilePath', status: 'error', cause : 'unknownError' }`

