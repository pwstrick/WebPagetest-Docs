# Mobile Testing

For mobile testing you have a few options for network connectivity:
+ Real mobile network with an actual carrier
+ WiFi with no traffic shaping
+ WiFi with a fixed traffic shaping profile
+ WiFi with per-test traffic shaping

This is what the mobile testing network looks like for the public instance of WebPagetest (at least as of this writing in 2014):

![](/assets/img/system/WebPagetest Mobile Network.png)

The different configuration build on top of each other:

### Devices
Testing with the new Node.js agent requires Android devices running 4.4 (Kit Kat) or later or iOS devices running a recent version of iOS (more iOS documentation and improved support coming soon).  In both cases the devices need to be rooted (jailbroken in the case of iOS) in order to be able to delete the browser cache.  For Android, Nexus and Motorola devices are known to work well and provide a wide range of capabilities.  Separate devices are required for testing portrait vs landscape (mostly an issue for tablet testing).

### Real mobile network with an actual carrier
