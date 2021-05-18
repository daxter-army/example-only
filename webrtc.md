# WebRTC - Web Real Time Communication

## Full documentation is awesome and available at [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)

## Connect to Media Devices
* In the index.html file (name is upto you) of your project
```html
    <head>
        <!-- all head data -->
    </head>
    <body>
        <!-- body data -->
        <video id='video' autoplay></video>
        <!-- body data -->
        <!-- before end of body tag, when all html elements are declared -->
        <!-- change src according to your folder structure -->
        <script src='./index.js' type="application/javascript"></script>
    </body>
```
### Promise based
* In index.js file
```js
    const constraints = {
        audio: true,    // permission for audio, if you dont want audio, dont mention it here
        video: true,    // there are various properties that you can pass in the video key value, are discussed below
    }

    navigator.mediaDevices.getUserMedia(constraints)
        .then(function(stream) {
            const videopPlayer = document.getElementById('video')
            videoPlayer.srcObject = stream
            videoPlayer.play()
        })
        .catch(function(error) {
            console.error('ERROR: navigator.mediaDevices.getUserMedia()')
            console.log('ERROR.MESSAGE: ', error.message)
            console.log('ERROR.NAME: ', error.name)
        })
```

### Async/Await based
* In index.js file
```js
    const constraints = {
        audio: true,    // permission for audio, if you dont want audio, dont mention it here
        video: true,    // there are various properties that you can pass in the video key value, are discussed below
    }

    // immediately invoked function expression
    // is called automatically with declaration
    // although you can use regular async function calls
    (async function() {
        // try catch blocks are recommended to use for handling any exceptions
        try {
            const stream = await navigator.mediaDevices.getUserMedia(constraints)
            const videoPlayer = document.getElementById('video')
            videoPlayer.srcObject = stream
            videoPlayer.play()
        }
        catch(error) {
            console.error('ERROR: navigator.mediaDevices.getUserMedia()')
            console.log('ERROR.MESSAGE: ', error.message)
            console.log('ERROR.NAME: ', error.name)
        }
    })()
```

### Video Constraints
* you can use
```js
    const supported = navigator.mediaDevices.getSupported()
    console.log(supported)
```
* to know all the supported constraints supported by your device's media components

```js
    const constraints = {
        // width of video stream
        width: { min: 640, ideal: 1920, max: 1920 },
        // height of video steam
        height: { min: 400, ideal: 1080 },
        // aspect ratio to be maintained
        aspectRatio: 1.777777778,
        // FPS (you know it!)
        frameRate: { max: 30 },
        // camera to be opened, "user" => front camera, "environment" => back camera
        facingMode: { exact: "user" },
        // for specific device to be used you can use deviceId like this
        facingMode: { deviceId: { exact: deviceId } },
    }
```
* deviceId can be retrieved from enumerateDevices()

## Connect to specific media device
```js
    (async function(){
        const devices = await navigator.mediaDevices.enumerateDevices()
        console.log(devices)
    })()

    // OR

    navigator.mediaDevices.enumerateDevices()
        .then(function(devices) {
            console.log(devices)
        })
        .catch(function(error) {
            console.log('ERROR: navigator.mediaDevices.enumerateDevices')
            console.log('ERROR.MESSAGE: ', error.message)
            console.log('ERROR.NAME: ', error.name)
        })

```

* Explaination:
```js
    navigator.mediaDevices.enumerateDevices()
```
* the above code gives you back an array containing the media (audio/video) devices available on your machine/device. It gives back you 4 values
*   **groupId** - random generated unique string, media on same devices will have same groupId, but in reality, they can be different, idk why 😅
*   **deviceId** - random generated unguessable id corresponding to every media device, is refreshed after every session
*   **label** - a label like front facing/back facing camera
*   **kind** - have two values => audioinput/videoinput

**NOTE!**
* Now here's a **CATCH**. In chrome (and chromium based browsers) you can run enumerateDevices() before getUserMedia() to get the labels of the attached media devices and can set your device acording to your project's need. But in Firefox, if you do so you will get back the device array but the labels will be empty due to security reasons. So in order to access device labels in firefox you must get permission first by running getUserMedia(), then after you can set the desired camera. However If you do so, you need to cancel/stop the previous stream, run getUserMedia() with the preferred settings and then again set the stream to your videoplayer, after getting device labels.

## Stop camera stream
* get all the streams from the camera and stop them, when you dont need them anymore
```js
    stream.getVideoTracks().forEach(function(track) {
        track.stop()
    })
```

**POINTS TO REMEMBER**
* you can not have two or more, simultaneously running instances of **navigator.mediaDevices.getUserMedia()** in your browser.
* if you want to do that, for any reason, it will work on chrome and chromium based browsers, but still firefox can throw error like
```js
    AbortError
    failed to start video
```
* when you don't want the camera stream anymore, stop the streaming, it is the recommended way of using webRTC.
* webRTC contains many experimental implementations that are still not-implemented/being-implemented in different browsers. Therefore, before using any feature, check its compatibility on the [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API) page, and the second source is [Caniuse](https://www.caniuse.com)