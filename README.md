### Slice Master 6000
### version 0.9.0
### Donald Beaudry (K1DBO)

-----------------------------------------------------------------


Take control of your Flex 6000 series radio's slice receivers with
Slice Master 6000.

Slice Master 6000's primary focus is on dynamically configuring and
launching CWSkimmer to work with the slice receivers in your radio.  A
CWSkimmer instance can be launched for any active slice so long as
it's panadapter assoicated with a DAX IQ channel.  Clicking on a
signal in the CWSkimmer window will cause the associated slice
receiver to change frequency.  Likewise, changing the frequency of a
slice receiver will cause the associated CWSkimemr to follow along.

# Getting Started

Download the latest release from

https://github.com/K1DBO/slice-master-6000/releases 

and open the zip file.  The executable is all you'll need and doesnt
require a formal installation.  You can run it from anywhere.  Windows
is likely to ask you to allow a firewall exception so Slice Master can
estabilish network connection with CWSkimmer and with your radio.

It is important to have already installed and configured
CWSkimmer. When Slice Master needs to create a new CWSkimmer instance
it will start with the default configuration.  If something goes wrong
with the configuration process, you can remove the newly created
config files.  They should be found in

C:\Users\your-user-name\AppData\Local\K1DBO 

and named Slice-A though Slice-H.

# Configuration

Once running Slice Master will present you with an tab for each slice
receiver supported by your radio.  Each tab gives you an option to
control when a CW Skimmer should be configured and launched for that
slice.  

Each slice also supports a "follow" option.  Use it to control make
the frequency of one slice follow the frequency of another slice.
That is, if slice B is set to follow slice A, anytime slice A changes
frequency, slice B will follow the change.  If you would like slice A
and slice B to always have the same frequency, you'll have to tell
slice to follow slice B and slice B to follow slice A.

The Settings tab lets you set the starting port number used by the CW
Skimmer instances.  Each CW Skimmer instance needs it's own port for
commumicating with Slice Master.  The instance associated with slice A
will use the port number specified in the settings tab.  Each slice
after A will use the next higher port number.  With the default port
number of 7300, a Flex 6300 will use ports 7300 and 7301 while a Flex
6700 will use ports 7300 through 7307.


# Trouble Shooting


On occasion, CW Skimmer will not start properly and display a
permissions dialog.  When this happens, you can shut down CW Skimmer
by selecting the tab for the slice and choosing 'never' as the launch
option.  Then, try restarting by selecting 'CW only' or 'When active'.

If a CW instance doesnt appear when you think it should, verify that
the slice receiver is active (visible somewhere in SmartSDR) and set
to CW mode if 'CW only' was selected as the launch option.  Also make
sure that the panadapter containing the slice has a DAX IQ channel
selected.








