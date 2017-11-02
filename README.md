# ros_notes
ROS notes, practical best practices


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

## Subscribers and publishers

* For publishers, check for number of subscribers first
* In node / nodelet init first setup, then start publishing, then start subscribing
  * If subscribing before everything is setup things only work sometimes
* Add a ``is init`` check in the main loop if we need some stuff being published first before we can run
* If using nodelets with a separate running thread, subscriber callbacks are asynchronous and need locking

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
* Make one set of launch files which always get used
  * One ``.launch`` which includes other ``.launch.xml``
  * If a separate / different launcher is needed it should be just re-using the ``.launch.xml``

# Links

* http://nerve.uml.edu/ros-2014/ROS%20Best%20Practices%20-%20Tully%20Foote.pdf
