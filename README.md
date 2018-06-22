# ros_notes
ROS notes, practical best practices
TEST

# Scribbles
## General

* Topics are non-blocking and __non-reliable__
  * Use services if non-streaming data
  
* Separate message package
* Seperate no-ros standalone package
  * Makes testing and verification so much easier
  * Forces low coupling

* Everything is SI units, if not specify it in the variable name

* Nodelet everything, if a standalone is needed to add a launcher with a mangager + just that nodelet

## C++ vs Python

* Multithreading
  * C++ by default single threaded (also sub and service callbacks)
    * Each sub / pub / service gets served once per spin()
  * Python uses separate threads for callback by default

## Subscribers and publishers

* For publishers, check for number of subscribers first
* In node / nodelet init first setup, then start publishing, then start subscribing
  * If subscribing before everything is setup things only work sometimes
* Add a ``is init`` check in the main loop if we need some stuff being published first before we can run
* If using nodelets with a separate running thread, subscriber callbacks are asynchronous and need locking
* Predefined stings in messages have to be without ``"``

## Dyn reconfigure

* Load from the rosparam server automatically
  * Parameter overwrite order: Dyn Config file, params loaded from launcher
  * Defaults should be edited in the param files loaded by the launcher
  * Changes of defaults in the Dyn Config file need a cmake triggering to take place
* Reconfigure callback gets triggered once when setting up dyn reconfigure client (in init usually)
  * No need to wait for params getting loaded after init, they are definitely loaded
* If in nodelet (and we have a processing thread) the reconfigure is async and needs locking

# Launchers

* There are always too many launch files
  * Don't get maintained and stop working at some point
* Parameter bash script execution hack: `<param name="$(anon hack)" command="<bash file> <args>">`
  * Gets executed before any launcher (all parameters get loaded first)
    * But after the roscore starts 
  

