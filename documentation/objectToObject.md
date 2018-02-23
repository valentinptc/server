# How to communicate from Object to Object
1. Introduction
2. Installation
3. QuickStart
4. Get 3D Matrix
5. Send and Receive Messages to all visible Objects
6. Calculate Distance and Orientation
7. Appendix


### 1. Introduction
For the Reality Editor, every Vuforia Target is a separate webpage (object). The Reality Editor provides APIs that allow all visible web pages to communicate to provide the look and feel of one big application while loading the content when needed. The following example shows two objects communicating their AR position. Each object uses this info to calculate and display distance and orientation.


### 2. Installation
1. Download the "Reality Editor" from the App Store
2. Install the latest version of https://nodejs.org
3. Download the **latest** server from https://github.com/valentinptc/server
4. Unpack the Server and install dependencies within the server folder: `npm install`
5. Start the Server with: `node server.js`

### 3. QuickStart
1. Make sure that your phone and your computer with the server is running on the same private Wifi network. The Reality Editor uses UDP Broadcasting to find the Server. (If Apple TV works in your network, the Reality Editor should work.)
2. If you are in a cooperate network you can  

2. Print out these two targets and point the Reality Editor at them.

#### Object A
![3D Object](https://raw.githubusercontent.com/valentinptc/server/master/objects/distanceA/target/target.jpg)

##### 2D Content
![3D Object](https://raw.githubusercontent.com/valentinptc/server/master/objects/distanceB/target/target.jpg)


### Get 3D Matrix
Subscribe to Projection and Model View Matrix:
   
    var obj = new HybridObject();
     obj.subscribeToMatrix();
     obj.addMatrixListener(function(m,p){
        // Place your code here
    });
  


### Send and Receive Messages to all visible Objects
To send a message to all objects:

    obj.sendGlobalMessage(msg);

Receive messages:
  
    obj.addGlobalMessageListener(function(msg){
        
    });
    

### Calculate Distance and Orientation
    var thisM = {x:0,y:0,z:0};
    var mV = {x:0,y:0,z:0};
    var m1 = null;
    var distance = 0;
    var angle = 0;
    var obj = new HybridObject();
    obj.subscribeToMatrix();
    obj.addMatrixListener(function(m,p){
        m1 = m;
        thisM.x =m[12];
        thisM.y =m[13];
        thisM.z =m[14];
        obj.sendGlobalMessage({object: realityObject.object, x:m[12],y:m[13],z:m[14]});
    });

    obj.addGlobalMessageListener(function(msg){

        mV.x =(thisM.x-msg.x);
        mV.y =(thisM.y-msg.y);
        mV.z =(thisM.z-msg.z);

        distance =   Math.sqrt(Math.pow(mV.x, 2) + Math.pow(mV.y, 2) + Math.pow(mV.z, 2));
        angle = Math.atan2(mV.x,mV.y)  - Math.atan2(m1[1], m1[5]) + Math.PI;

    });
    

### Appendix
http://realityeditor.org/reference-javascript/