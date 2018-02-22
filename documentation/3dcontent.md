# How to add 3D Content in to the Reality Editor
&nbsp;1. Introduction
<br>&nbsp;2. Importent to Know
<br>&nbsp;3. Logic Block Components
<br>&nbsp;4. Program Module (index.js) 
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1. generalProperties
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2. setup
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3. render
<br>&nbsp;5. Icon and Label
<br>&nbsp;6. Index.html


### 1. Introduction
The Reality Editor allows to load multiple webpages in to AR parallel.
If you just want to add 2D content, you upload html webcontent to the Reality Editor server and you are ready to go. 

If you want to load 3D content, the Reality Editor provides API's that allows you to take control of the 3D transformations by your self. This tutorial explains how to build a simple demonstration using threejs. 


### 2. Installation
1. Download the Reality Editor from the App Store
2. Install the latest version for https://nodejs.org
3. Download the server from https://github.com/valentinptc/server
4. Unpack the Server and install dependencies within the server folder: `npm install`
5. Start the Server with: `node server.js`

### 3. QuickStart
1. Make sure that your phone and your computer with the server is running within the same private Wifi network. The Reality Editor uses UDP Broadcasting to find the Server. (If Apple TV works in your network, the Reality Editor should work.)

2. Open the Reality Editor and point it at these two Targets



