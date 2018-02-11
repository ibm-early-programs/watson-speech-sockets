# watson-speech-sockets
Example showing how to make use of Watson Speech To Text with Web Sockets in Node-RED

## Usage
This html and flow show how to use the Watson Speech to Text Node-RED Node
in Web Socket mode.
The usage is very similar to how to use the Watson Speech to Text service in Web Socket
mode. The format of the actions and data is identical. The significant difference is that
a token is not needed as the node takes care of that.

When running in socket streaming mode the Node expects to be listening and writing to
Node-RED websocket nodes. If you use the html as is, then the socket is on `/ws/stt`

## node-red-node-watson
The feature that this flow uses was released in version 0.6.3 of the node-red-node-watson. As a consequence you will need at least that version.

## html
The sample html and javascript is a cut-down to the essentials only of the standard
HTML5 audio capture and web socket examples. 


## Sample flow
````
[{"id":"f6e79.e431d187","type":"websocket in","z":"3161c0d5.473bd","name":"STT In","server":"6c11fd94.70e4c4","client":"","x":136,"y":256,"wires":[["ed31c194.d3413","5764654b.ca1cdc"]]},{"id":"5764654b.ca1cdc","type":"watson-speech-to-text","z":"3161c0d5.473bd","name":"STT with Sockets","alternatives":1,"speakerlabels":true,"smartformatting":true,"lang":"en-US","langhidden":"en-US","langcustom":"NoCustomisationSetting","langcustomhidden":"NoCustomisationSetting","band":"NarrowbandModel","bandhidden":"NarrowbandModel","password":"","payload-response":true,"streaming-mode":true,"default-endpoint":true,"service-endpoint":"https://stream.watsonplatform.net/speech-to-text/api","x":345.5,"y":257,"wires":[["bc9b0276.805e5","6f94bbcc.9939a4"]]},{"id":"6f94bbcc.9939a4","type":"websocket out","z":"3161c0d5.473bd","name":"","server":"6c11fd94.70e4c4","client":"","x":561,"y":258,"wires":[]},{"id":"ed31c194.d3413","type":"debug","z":"3161c0d5.473bd","name":"Socket Input","active":true,"console":"false","complete":"payload","x":302,"y":335,"wires":[]},{"id":"bc9b0276.805e5","type":"debug","z":"3161c0d5.473bd","name":"STT Output","active":true,"console":"false","complete":"true","x":543,"y":335,"wires":[]},{"id":"b3ee6346.b3ca7","type":"http in","z":"3161c0d5.473bd","name":"","url":"/transcribe","method":"get","upload":false,"swaggerDoc":"","x":134,"y":151,"wires":[["7c2be8da.909278"]]},{"id":"7c2be8da.909278","type":"http request","z":"3161c0d5.473bd","name":"","method":"GET","ret":"txt","url":"https://raw.githubusercontent.com/ibm-early-programs/watson-speech-sockets/master/transcriber.html","tls":"","x":324,"y":152,"wires":[["dc20d5ea.29e6c8"]]},{"id":"dc20d5ea.29e6c8","type":"function","z":"3161c0d5.473bd","name":"Reset msg.headers","func":"msg.headers = {};\nreturn msg;","outputs":1,"noerr":0,"x":521.5,"y":153,"wires":[["a05eb481.1bd6e8"]]},{"id":"a05eb481.1bd6e8","type":"http response","z":"3161c0d5.473bd","name":"","statusCode":"","headers":{},"x":674.5,"y":74,"wires":[]},{"id":"6c11fd94.70e4c4","type":"websocket-listener","z":"","path":"/ws/stt","wholemsg":"false"}]
````
