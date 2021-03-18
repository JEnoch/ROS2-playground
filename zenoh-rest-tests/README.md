# Some examples of using Zenoh REST API for ROS2

## **Requirements**

 * A [zenoh router](http://zenoh.io/docs/getting-started/quick-test/)
 * The [zenoh/DDS bridge](https://github.com/eclipse-zenoh/zenoh-plugin-dds#trying-it-out)
 * [jscdr](https://github.com/atolab/jscdr)
   (automatically retrieved by your Web browser)
 * ROS2 [turtlesim](http://wiki.ros.org/turtlesim) (or any other robot able to receive Twist messages...)

-----
## **Usage**

### ros2-teleop.html

A simple teleop web page allowing to publish Twists and to subscribe to Logs
via the zenoh REST API, bridged to ROS2.

 1. Start the turtlesim:
      ```bash
      ros2 run turtlesim turtlesim_node
      ```
 2. Start the zenoh router with a memory storage (to store the HTML page and serve it via the REST API):
      ```bash
      zenohd --mem-storage="/demo/**"
      ```
 3. Start the zenoh/DDS bridge:
      ```bash
      dzd
      ```
 4. Put the ros2-teleop.html into zenoh under `/demo/ros2-teleop`
      ```bash
      curl -X PUT -H 'Content-Type: text/html' -d "`cat ros2-teleop.html`" http://localhost:8000/demo/ros2-teleop
      ```
 5. Open http://localhost:8000/demo/ros2-teleop
 6. Use the arrows keys to drive the robot

**Notes**:

Both zenoh router and Teleop can be deployed in different networks than the robot. Only the zenoh/DDS bridge has to run in the same network than the robot (for DDS communication via UDP multicast).  
For instance, you can:
 * deploy the zenoh router in a cloud on a public IP with port 7447 and 8000 open
 * configure the zenoh bridge to connect this remote zenoh router:
     ```bash
     dzd -m client -e tcp/<cloud_ip>:7447
     ```
 * Put and get the HTML page on `http://<cloud_ip>:8000/demo/ros2-teleop`


