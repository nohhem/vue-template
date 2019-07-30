<template>
  <div id="app">
    <img width="25%" src="./assets/logo.png">
    <HelloWorld msg="Hello Vue in CodeSandbox!" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    HelloWorld
  }
};
console.log("Running JavaScript!");
/**
 * Create HTTP server.
 */

var http = require('http');
var port = process.env.PORT || 3000;
app.set('port', port);
var server = http.createServer(app);
server.listen(port);

/**
 * Create IO connection
 */
console.log("Running JavaScript!1");
var io = require('socket.io').listen(server);


/**
 * Create SerialPort connection
 */
console.log("Running JavaScript!2");
var SerialPort = require("serialport").SerialPort

// creates a new serial port connection, in this case to port COM5
// Windows users: to find out which port is used, go to: Device Manager -> Ports (COM & LPT)
var serialPort = new SerialPort("COM5", {
	baudrate: 9600
}, false); // this is the openImmediately flag [default is true]


serialPort.open(function (error) {
	if ( error ) {
		console.log('failed to open: ' + error);
	} else {
		console.log('serial port opened');
		serialport_opened = true;

		// get data from connected device via serial port
		serialPort.on('data', function(data) {
			// get buffered data and parse it to an utf-8 string
			data = data.toString('utf-8');
			// you could for example, send this data now to the the client via socket.io
			// io.emit('emit_data', data);
		});

		serialPort.on('error', function(data) {
			console.log('Error: ' + data);
		})
	}
});

// sends data to the connected device via serial port
function writeAndDrain (data, callback) {
	// flush data received but not read
	serialPort.flush();

	// write/send data to serial port
	serialPort.write(data, function (error) {
		if(error){console.log(error);}
		else{
			// waits until all output data has been transmitted to the serial port.
			serialPort.drain(callback);      
		}
	});
}

io.on('connection', function (socket) {
	console.log('user connected');
	socket.on('disconnect', function(){
		console.log('user disconnected');
	});
	socket.on('get_data', function(data) {
		// send data to the connected device via serial port
		writeAndDrain(data, null);
	})
});

</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
