# watson-speech-sockets
Example showing how to make use of Watson Speech To Text with Web Sockets in Node-RED

## Usage
This html and flow show how to use the Watson Speech to Text Node-RED Node
in Web Socket mode.
The usage is very similar to how to use the Watson Speech to Text service in Web Socket
mode. The format of the actions and data is identical. The significant difference is that
a token is not needed as the node takes care of that.

The Echo flow shows how to extend the Speech to Text flow with the response going through Watson Text to Speech and being played back in the browser.

When running in socket streaming mode the Node expects to be listening and writing to
Node-RED websocket nodes. If you use the html as is, then the socket is on `/ws/stt`

## node-red-node-watson
The feature that this flow uses was released in version 0.6.3 of the node-red-node-watson. As a consequence you will need at least that version.

## html
The sample html and javascript is a cut-down to the essentials only of the standard
HTML5 audio capture and web socket examples.


## Sample flow - Transcriber
````
[{"id":"f6e79.e431d187","type":"websocket in","z":"3161c0d5.473bd","name":"STT In","server":"6c11fd94.70e4c4","client":"","x":136,"y":256,"wires":[["ed31c194.d3413","5764654b.ca1cdc"]]},{"id":"5764654b.ca1cdc","type":"watson-speech-to-text","z":"3161c0d5.473bd","name":"STT with Sockets","alternatives":1,"speakerlabels":true,"smartformatting":true,"lang":"en-US","langhidden":"en-US","langcustom":"NoCustomisationSetting","langcustomhidden":"NoCustomisationSetting","band":"NarrowbandModel","bandhidden":"NarrowbandModel","password":"","payload-response":true,"streaming-mode":true,"default-endpoint":true,"service-endpoint":"https://stream.watsonplatform.net/speech-to-text/api","x":345.5,"y":257,"wires":[["bc9b0276.805e5","6f94bbcc.9939a4"]]},{"id":"6f94bbcc.9939a4","type":"websocket out","z":"3161c0d5.473bd","name":"","server":"6c11fd94.70e4c4","client":"","x":561,"y":258,"wires":[]},{"id":"ed31c194.d3413","type":"debug","z":"3161c0d5.473bd","name":"Socket Input","active":true,"console":"false","complete":"payload","x":302,"y":335,"wires":[]},{"id":"bc9b0276.805e5","type":"debug","z":"3161c0d5.473bd","name":"STT Output","active":true,"console":"false","complete":"true","x":543,"y":335,"wires":[]},{"id":"b3ee6346.b3ca7","type":"http in","z":"3161c0d5.473bd","name":"","url":"/transcribe","method":"get","upload":false,"swaggerDoc":"","x":134,"y":151,"wires":[["7c2be8da.909278"]]},{"id":"7c2be8da.909278","type":"http request","z":"3161c0d5.473bd","name":"","method":"GET","ret":"txt","url":"https://raw.githubusercontent.com/ibm-early-programs/watson-speech-sockets/master/transcriber.html","tls":"","x":324,"y":152,"wires":[["dc20d5ea.29e6c8"]]},{"id":"dc20d5ea.29e6c8","type":"function","z":"3161c0d5.473bd","name":"Reset msg.headers","func":"msg.headers = {};\nreturn msg;","outputs":1,"noerr":0,"x":521.5,"y":153,"wires":[["a05eb481.1bd6e8"]]},{"id":"a05eb481.1bd6e8","type":"http response","z":"3161c0d5.473bd","name":"","statusCode":"","headers":{},"x":674.5,"y":74,"wires":[]},{"id":"6c11fd94.70e4c4","type":"websocket-listener","z":"","path":"/ws/stt","wholemsg":"false"}]
````

## Sample flow - Echo
````
[{"id":"e3f8d541.21f6c8","type":"websocket in","z":"e37ede3b.4935d","name":"STT In","server":"7b210ab2.624a64","client":"","x":90,"y":240,"wires":[["985378ec.5cfde8","36abfdc1.152a02"]]},{"id":"c9b4d0.74788b3","type":"websocket out","z":"e37ede3b.4935d","name":"","server":"7b210ab2.624a64","client":"","x":540,"y":380,"wires":[]},{"id":"1bbe858b.7adcda","type":"debug","z":"e37ede3b.4935d","name":"STT Output","active":false,"console":"false","complete":"true","x":470,"y":240,"wires":[]},{"id":"36abfdc1.152a02","type":"watson-speech-to-text","z":"e37ede3b.4935d","name":"","alternatives":1,"speakerlabels":true,"smartformatting":true,"lang":"en-US","langhidden":"en-US","langcustom":"NoCustomisationSetting","langcustomhidden":"NoCustomisationSetting","band":"NarrowbandModel","bandhidden":"NarrowbandModel","password":"","payload-response":true,"streaming-mode":true,"default-endpoint":true,"service-endpoint":"https://stream.watsonplatform.net/speech-to-text/api","x":260,"y":240,"wires":[["1bbe858b.7adcda","b3d54733.4240a8"]]},{"id":"985378ec.5cfde8","type":"debug","z":"e37ede3b.4935d","name":"Socket Input","active":false,"console":"false","complete":"payload","x":250,"y":180,"wires":[]},{"id":"52b0767d.eadd88","type":"watson-text-to-speech","z":"e37ede3b.4935d","name":"","lang":"en-GB","langhidden":"en-GB","langcustom":"NoCustomisationSetting","langcustomhidden":"","voice":"en-GB_KateVoice","voicehidden":"","format":"audio/wav","password":"","payload-response":true,"default-endpoint":true,"service-endpoint":"https://stream.watsonplatform.net/text-to-speech/api","x":520,"y":320,"wires":[["c9b4d0.74788b3"]]},{"id":"b3d54733.4240a8","type":"function","z":"e37ede3b.4935d","name":"Check for transcription","func":"if (msg.payload && msg.payload.results && \n      msg.payload.results[0].final) {\n    var newMsg = {payload: msg.payload.results[0].alternatives[0].transcript};  \n      return [newMsg, msg];  \n}\nreturn [null, msg];","outputs":"2","noerr":0,"x":300,"y":320,"wires":[["52b0767d.eadd88","cd61028f.b3131"],["c9b4d0.74788b3"]]},{"id":"cd61028f.b3131","type":"debug","z":"e37ede3b.4935d","name":"TTS Input","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","x":500,"y":280,"wires":[]},{"id":"d9476d53.f1aee","type":"http request","z":"e37ede3b.4935d","name":"","method":"GET","ret":"txt","url":"https://raw.githubusercontent.com/ibm-early-programs/watson-speech-sockets/master/echo.html","tls":"","x":290,"y":120,"wires":[["27052279.05f52e"]]},{"id":"27052279.05f52e","type":"function","z":"e37ede3b.4935d","name":"Reset msg.headers","func":"msg.headers = {};\nreturn msg;","outputs":1,"noerr":0,"x":490,"y":120,"wires":[["9c92163e.0e0c98"]]},{"id":"9c92163e.0e0c98","type":"http response","z":"e37ede3b.4935d","name":"","statusCode":"","headers":{},"x":690,"y":120,"wires":[]},{"id":"384d1111.7ac97e","type":"http in","z":"e37ede3b.4935d","name":"","url":"/transcribe10","method":"get","upload":false,"swaggerDoc":"","x":110,"y":120,"wires":[["d9476d53.f1aee"]]},{"id":"7b210ab2.624a64","type":"websocket-listener","z":"","path":"/ws/stt10","wholemsg":"false"}]
````
