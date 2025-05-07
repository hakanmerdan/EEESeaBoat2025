# EEERover starter code

This project is an example of setting up WiFi communication with the Metro M0 Express module using HTTP.
The microcontroller connects to a WiFi network and acts as a simple web server, returning a basic web page when it receives an HTTP request.
It also acts on certain paths in the HTTP request to switch an LED on and off when the client clicks buttons on the webpage.

## Building the project

Additional libraries are needed to use Arduino IDE with the Metro M0 board and WiFi module. [Follow the instructions from Adafruit](https://learn.adafruit.com/adafruit-metro-m0-express/arduino-ide-setup) to add support for the Metro M0 Express board in Arduino IDE. Alternatively, you can build the project in [Visual Studio Code using PlatformIO](../metro-starter-pio/README.md).
			
The WiFi communication functions are provided by the WiFiWebServer library, which also must be installed with the Library Manager. The library installs its own set of example sketches and has an [API reference](https://github.com/khoih-prog/WiFiWebServer). The demonstration code in this repository is based on these examples.

## Network configuration

The code is configured to connect to the EEERover network in the lab. You can give a different WiFi network and password, such as a mobile hotspot, by changing the appropriate lines in the code. You won't be able to connect to the College WiFi network, which uses a login. When the code starts it reports its IP Address on the serial monitor. You can connect to the microcontroller from a computer on the same WiFi network by typing this address into a web browser. WiFi functions are provided by the [WiFi101](https://www.arduino.cc/reference/en/libraries/wifi101/) library.

You may need to connect to the microcontroller when it is not connected with a USB cable, so you won't see the message reporting the IP address. You can fix the IP address to a certain value by setting `groupNumber` to your group number. The IP address will be `192.168.0.n+1`. You can only do this on the EEERover network because it has been configured to allow a certain range of static IPs.

## HTTP server

The [WiFiWebServer](https://github.com/khoih-prog/WiFiWebServer) library is used to handle HTTP requests from WiFi clients.
HTTP is the protocol used to access websites and it is used here because it allows the creation of a simple user interface using a web browser.

The server is configured in the `setup()` function using calls to `server.on()` to define what happens when requests are received.
Each HTTP request has a URI, which is the path of the resource that is requested. The server responds to three different URIs:
 - `/` The root path, e.g. `http://192.168.0.10/`
 - `/on` A request to turn the LED on, e.g. `http://192.168.0.10/on`
 - `/off` A request to turn the LED off, e.g. `http://192.168.0.10/off`

A different function is called when each request is received. A request to the root path returns the html code for the user interface. `/on` and `/off` set the LED and the reponse is the state of the LED, either `ON` or `OFF`.

### The web interface

The webpage built into the starter code is a basic user interface based on JavaScript and HTTP requests. `<button>` tags are used to define two buttons, which are formatted with some basic styling defined in the `<style>` section. Each button triggers a JavaScript function that is defined in the `<script>` section. In these functions, a [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest) object is used to send a HTTP request with its `send()` and `open()` methods. 

An inline function is assigned to `xhttp.onreadystatechange`, which is called whenever a response is received to a HTTP request. The function updates the LED state text displayed on the page with the contents of the HTTP response. Remember that all the JavaScript functions are executed on the client web browser, not on the microcontroller module. You could host or store the webpage somewhere else entirely - you would just need to modify the URLs for the on and off HTTP requests to include the full hostname (IP address) of the microcontroller.

## Next steps

The code gives an example of sending information in both directions between the microcontroller and the remote client.
It can be extended to include all the data you need to control your rover and get data back.
