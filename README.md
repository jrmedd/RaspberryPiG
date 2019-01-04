## About

**RaspberryPiG** is a workshop project to demonstrate basic IoT stuff using [Node-RED](https://nodered.org) and [Raspberry Pi](https://raspberrypi.org/). He lights up when you receive emails & tweets, and glows blue if it's raining where he is.


### Why a pig?

The reasons are twofold:
1. February 5th 2019 marks the beginning of the Year of the Pig on the Chinese calendar.
2.  The Museum of English Rural Life once asked a question about making pigs fit for the 21st century, [this was my answer](https://twitter.com/jamesmedd/status/1045214372501573632).

### Why a Pi?

It's affordable, small, well-documented, easy to use, made in the UK etc. There are cheaper, more lightweight ways of doing IoT workshops, but for very short sessions, the plug-and-play nature of the Pi (and the fact it runs [Node-RED](https://nodered.org)) is nice and easy.

### Why Node-RED?

Short workshop sessions put pressure on the student and the teacher. **Absolute beginners** will not write exciting code in a short session, unless most of it is copy/pasted or typed out by students looking at a slide projection. This obfuscates coding and makes it difficult for a teacher to quickly debug if there are formatting/syntax errors.

Node-RED *does* somewhat obfuscate the complexity of the code that powers it, but it is a complete system and useful playground for newcomers to experiment in. I think it's a useful tool for making rapid prototypes too and, as a teacher, it's easy for me to scan my eyes over to make sure students are keeping up.

## How does it work?

There's a Node-RED flow that students can construct, or that you can import directly from the flow .json file.

### Blinkt

The Blinkt node executes a Python script that receives data from Node-RED via the standard input stream in order to illuminate the LED strip. The message payload sent to the node should be a comma-separated RGB value **e.g. '255, 0, 0' for red**. The message topic will determine how the RGB message is parsed. These messages include 'all', 'clear', 'pixel.*index*' (where index is the pixel you want to address), 'animate' and 'pulse'.

### Twitter

The [Twitter node for Node-RED](https://www.npmjs.com/package/node-red-node-twitter) needs populating with developer credentials (**details in requirements**). This node can be configured to respond whenever a particular user tweets, when specified search terms are tweeted, or when the authenticated user receives a direct message.

### Weather

The PiG's weather sensing is supported by two APIs. [IP-API](http://ip-api.com) provides location data based on the IP address of the PiG. The longitude and latitude are passed to the [Dark Sky](http://darksky.net/) API to give a forecast for the day's weather at that location. If 'rain' is found in the weather summary, a message is sent to the Blinkt strip to turn on the blue LEDs. This call is made every 15 minutes, and is stored as a flow variable so that it can be recalled after any other LED notifications.

### Email

The [email node for Node-RED](https://flows.nodered.org/node/node-red-node-email) needs populating with a server address and access credentials (I've tested successfully with Gmail, but any POP3/IMAP server should suffice). The node will check a specified inbox at a specified frequency and return unread emails. **'Disposition' should be set to 'None' if you don't want it to delete every new email it receives**. 

The node is connected to a 'change' node that will output an LED animation message for every new email it receives. After a delay (determined by the number of unread emails found) the stored weather condition will be re-triggered, setting the LEDs back to blue if rain is forecast.

### Installing additional nodes

Nodes can be installed via one of two methods. The simplest is the 'Manage palette' section accessed via Node-RED's menu. This is populated by nodes published on [npm](https://www.npmjs.com/) that have the 'node-red' keyword. Note that mine and Pimoroni's nodes currently don't use this keyword (I'm not at a point where I feel it could be easily understood outside of the context of this workshop).

The other method of installing nodes (required by my nodes and Pimoroni's nodes) is to clone the node repos into the `/home/pi/.node-red/nodes` directory, which gets searched each time Nod-RED is started.

### Requirements

* This project is designed to run on a Raspberry Pi (I use the [Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w/)) with a [Blinkt](https://shop.pimoroni.com/products/blinkt) LED strip sat on top
* [Node-RED](https://nodered.org) needs to be installed.
* The Blinkt node comes from [Pimoroni](https://github.com/pimoroni/node-red-nodes) although I've made [some changes to add animations](https://github.com/jrmedd/node-red-nodes)
* The Blinkt Node depends on [Pimoroni's Blinkt library](https://github.com/pimoroni/blinkt)
* To make choosing LED colours a bit easier, I've created a [colour-picker node for Node-RED](https://github.com/jrmedd/node-red-contrib-colour-picker)
* Weather data comes from [Dark Sky](https://darksky.net), which you need an API key for. **1000 calls can be made per day for free**. The easiest way to use Dark Sky in Node-RED is with [node-red-node-darksky](https://www.npmjs.com/package/node-red-node-darksky)
* The Twitter node needs developer credentials that you can generate in [Twitter's developer portal](https://developer.twitter.com). After registering your application, you find your Consumer API keys and Access Token from the 'Keys and tokens' section of your app's configuration

## Raspberry PiG case

![Pig Image](https://github.com/jrmedd/RaspberryPiG/raw/master/RaspberryPiG.png)

The Raspberry PiG case is designed to accommodate a Raspberry Pi Zero W with a Blinkt strip on top (you may be able to squeeze a battery in there too, but there's a cable opening). It works best 3D-printed in a translucent material so that the light from the LEDs diffuses and illuminates the entire pig.

![Pig Bolts](https://github.com/jrmedd/RaspberryPiG/raw/master/RaspberryPiG_bolts.png)

I use M3 or M2.5 nylon nuts, bolts, and standoffs to mount the Pi using the printed holes.

