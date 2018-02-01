# How to use Hardware Interfaces

### Introduction
The hardwareInterface is how the Reality Server communicates with the physical world. If you want to connect to an Arduino board, there is an Arduino hardware interface. If you want to talk to Philips Hue lights, there is a hardware interface and so on.

### Quick Start
The hardware interfaces are stored in the folder `hardwareInterfaces/`. Each hardwareInterface is hosted in a folder with its name and an `index.js` file.

If you want to activate a hardware interface you have to open the index.js file within your hardware interface folder and set the first variable to `true`.

```
exports.enabled = true; 
```
Now when you start the server with `node server.js` the hardware interface is activated and can be used.

Hardware interface like wedo2 (Lego WeDo2.0) and hueStart (Interface for Philips hue lights) automatically generate objects and nodes based on the device found via Bluetooth (wedo2) or in the local network (Philips Hue)

Once your Hardware interface is activated you should configure your objects target files as well the AR-GUI using the developer FrontEnd interface. Once your server is started, you can find the FrontEnd interface with this address: `localhost:8080`

### Arduino Uno hardware interface
The Arduino Uno hardware interface is a special interface that comes with an additional Arduino library. Once you have activated the Arduino Uno hardware interface, you can use that library to program new objects and nodes as well send and receive data.

How to:
Copy the `HybridObject` folder from the Arduino Uno hardwareInterface folder into your Arduino libraries folder. 

In your Arduino code instantiate the Hybrid Objects library as followed:

```
#include <HybridObject.h>
HybridObject obj;
```

| API | functionality | example |
|:-- |:-- |:-- |
| `obj.developer()`| If called, the developer() function enables you to access all developer functionality. It allows you to move and scale user interfaces within the Reality Editor and it gives you access to the Developer Web-Page. Only call developer in the Arduino setup function. | <pre>void setup() {<br>obj.developer();<br>}</pre>|
|`obj.add([object], [node])` |With the .add() function you can add new nodes to your object. Nodes can be input, output or both. The direction is not relevant. The added node will be represented in the Reality Editor. | <pre>void setup() {<br>obj.developer();<br> obj.add("object", "led");<br> obj.add("object", "sensor");<br>}</pre>|
|`obj.read([object], [node])` |Within the Arduino loop() function you can use .read() to read data from a node. All values are floating point in the range between 0.0 and 1.0. |<pre>void setup() {<br> obj.developer();<br> obj.add("myObject", "led");<br>}<br>void loop() {<br> int input = obj.read("myObject", "led");<br>}</pre>| |
|`obj.write([object], [node], [value])` | Within the Arduino loop() function you can use .write() to write data to a node. All values are floating point in the range between 0.0 and 1.0. |<pre>void setup(){<br> obj.developer();<br> obj.add("myObject", "led");<br>}<br>void loop() {<br> int output = 123;<br> obj.write("myObject", "sensor", output);<br>}</pre>|
|`obj.map([value], [min], [max])` | Since all data is floating point between 0.0 and 1.0, the map function scales your data in to this scale. For example if you have a range between 0 and 100 and your value is 50 map() will return 0.5. |<pre>void setup() {<br>}<br>void loop() {<br> int input = 123;<br> float output = obj.map(input, 0,200);<br>}</pre>|

### Make your own Hardware Interface
1. To make your own hardware interface, create a new folder with your hardware interface name within the `hardwareInterfaces` folder.
2. Create an `index.js` file with the following structure:

```
exports.enabled = false;

if (exports.enabled) {
    var server = require(__dirname + '/../../libraries/hardwareInterfaces');

// place your hardware interface code here.
}
```

You can use the following API to program your hardware interface:

| API | functionality | example |
|:-- |:-- |:-- |
|`enableDeveloperUI([boolean])`|Enables the developer mode for all objects and enables the developer web interface.| <pre>server.enableDeveloperUI(true);<br></pre>|
|`addNode(object, node, type)`| Adds a new node to an object. If the object does not jet exist, the object is created as well.| <pre>server.addNode("legoCar", "sensor", "node");</pre>|
|`renameNode(object, oldNode, newNode)`| Renames a node within an object. This change is pushed in realtime to the Reality Editor and can help with plug and play interfaces. | <pre>server.renameNode("legoCar", "sensor", "motor");</pre>|
|`activate(object)`| Activates an object to be processed and broadcasted. | <pre>server.activate("legoCar");</pre>|
|`deactivate(object)`| Deactivates an object so its data is no longer processed and broadcasred to the Reality Editor.| <pre> server.deactivate("legoCar");</pre>|
|`write(object, node, value,`<br>` mode, unit, unitMin, unitMax)`| This function writes the values passed from the hardware interface to the Reality Server. `Mode` specifies the datatype of value. Currently the mode is `f`for floatingpoint. Mode, unit, unitMin, and unitMax are currently optional, and will respectively take on default values of 'f', false, 0, and 1.| <pre> server.write("legoCar", "motor", value, "f");</pre>|
|`addReadListener(object, node, CB)`| This function attaches a callback `CB` to an objects's node to listen for new values it receives and process them accordingly.| <pre>server.addReadListener("legoCar", "sensor",<br> function (data) {<br>  console.log(data.value);<br>});</pre>|
|`removeReadListeners(object)`|Removes the callbacks attached to an object by addReadListener.|<pre>server.removeReadListeners("legoCar");</pre>|
|`addEventListener(option, callBack)`|Listens for specific events from the server. At this moment, option can be either `reset` or `shutdown`.|<pre>server.addEventListener('shutdown',<br> function() {<br>  console.log('shutting down');<br>});</pre>|
|`map(value, in_min, in_max,`<br>` out_min, out_max)`|Remaps the value of x from the range of `[in_min, in_max]` to the range of `[out_min, out_max]`|<pre>var scaledValue = <br>server.map(data.value, 0, 1, -100, 100);</pre>| 