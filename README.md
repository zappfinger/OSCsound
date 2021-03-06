#OSCsound

## Introduction

This is a simple script for running Csound and starts an OSC server for rapid live Csound application development.  The server
takes in the following messages:

* /sco - 1 argument: score:string
* /cc - 2 arguments: channelName:string, value:float
* /cc/someChannelName - 1 argument: value: float
* /quit - 0 arguments

Score text sent to /sco will be read in by Csound using the InputMessage method on CsoundPerformanceThread. Values sent to /cc will call SetChannel on the Csound object with the given channelName and float value.  An alternative handler is also available that will read a "/cc/someChannelName" address and single float argument for setting channels.  The /quit message will shutdown the server and Csound and exit the program.

The workflow for using this script is:

1. Create Csound CSD.  The CSD design should be such that when it loads it should be ready to use realtime events.  This means that you might create a number of instruments that are always-on and configured to do things like effects processing and mixing. The CSD could have instruments designed to be triggered by realtime score events, and instruments can expect to read in values from channels using the chnget opcode. 
2. Design a client OSC Interface or program that will send /sco, /cc, and /quit messages.  
3. Start the server, start the client, perform music in realtime.

Also included with this project is an ipython notebook (OSCMessageTest.ipynb).  This notebook includes test scripts for sending score, cc, and quit messages. 

## Running

To run the script, you can use:

    python OSCsound.py path_to_csd_file.csd

You can also optionally give what port the OSC Server should run on using the --port commandline flag. The default port is 7110. 

You can always get more information by using:

    python OSCsound.py -h

which will display information such as:

    usage: OSCsound [-h] [-p PORT] csd_file

    OSCsound - version 0.1

    positional arguments:
      csd_file              Csound CSD File to run

    optional arguments:
      -h, --help            show this help message and exit
      -p PORT, --port PORT  Server Port, defaults to 7110

## License

This project is released under the LGPL v2.1 license.  Plese view the LICENSE file for more information. 

## Credits

This script uses the excellent pyOSC library Artem Baguinski. The OSC.py file is included with this repository for convenience.  It's license file is LICENSE-pyOsc. For more information about pyOSC, please go to https://gitorious.org/pyosc. 
