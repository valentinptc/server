# How to add 3D Content in to the Reality Editor
1. Introduction
2. Installation
3. QuickStart
4. 3D Content
5. Adding your own Object / Marker / Content


### 1. Introduction
The Reality Editor allows loading multiple web pages as Augmented Reality content. If you just want to add 2D content, you upload normal HTML web content to the Reality Editor server. 

If you want to load 3D content, the Reality Editor provides API's that allows you to take control of the 3D transformations. This tutorial explains how to build a simple demonstration using threejs. 


### 2. Installation
1. Download the "Reality Editor" from the App Store
2. Install the latest version of https://nodejs.org
3. Download the server from https://github.com/valentinptc/server
4. Unpack the Server and install dependencies within the server folder: `npm install`
5. Start the Server with: `node server.js`

### 3. QuickStart
1. Make sure that your phone and your computer with the server is running on the same private Wifi network. The Reality Editor uses UDP Broadcasting to find the Server. (If Apple TV works in your network, the Reality Editor should work.)

2. Open the Reality Editor and point it at these two Targets:

####3D Content
![3D Object](https://raw.githubusercontent.com/valentinptc/server/master/objects/3DBall/target/target.jpg)

#####2D Content
![3D Object](https://raw.githubusercontent.com/valentinptc/server/master/objects/YouTubeVideo/target/target.jpg)


### 2D Content
The 2D example is a video live feed embedded in to html. You can find the example here: `serverFolder/objects/YouTubeVideo/index.html`


### 3D Content
The 3D example you just saw was created with threejs. You can find the HTML content with comments in the following folder: `serverFolder/objects/3DBall/index.html`.

The following steps are required to clue threejs to a target:

1. Instantiate the Reality Editor Library `hybridObj = new HybridObject();`
1. Tell the Reality Editor to draw the content fulscreen and ignore the 2D transformations `hybridObj.setFullScreenOn();`
2. Subscribe to the transformation matrices `hybridObj.subscribeToMatrix();`
3. Add a listener for the matrices: `hybridObj.addMatrixListener(renderScene);` 
4. The Listenser will be called 30 times a second synchronized with the video framerate. It should be used to redraw the content.

The matrix listener provides the modelViewMatrix and projectionMatrix.
Threejs uses `matrix.set(<matrix>)` to apply the matrices. The projection matrix is applied to `camera.projectionMatrix`once and the modelViewMatrix is applied to `threejsContainerObj.matrix` each frame.


### Adding your own Object / Marker / Content
Follow the tutorial on this page:
http://realityeditor.org/building-objects-101/

(Scrall down to: **Adding a Target**)
