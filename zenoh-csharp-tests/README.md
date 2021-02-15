# Some examples of using Zenoh C# API for ROS2

## **Requirements**

 * A [zenoh router](http://zenoh.io/docs/getting-started/quick-test/)
 * The [zenoh/DDS bridge](https://github.com/eclipse-zenoh/zenoh-plugin-dds#trying-it-out)
 * [zenoh-csharp](https://github.com/eclipse-zenoh/zenoh-csharp) 
   (automatically retrieved by .NET from [NuGet](https://www.nuget.org/packages/Zenoh))
 * [CSCDR](https://github.com/atolab/CSCDR)
   (automatically retrieved by .NET from [NuGet](https://www.nuget.org/packages/CSCDR))
 * ROS2 [turtlesim](http://wiki.ros.org/turtlesim) (or any other robot able to receive Twist messages...)

-----
## **Usage**

### Ros2Teleop

A simple teleop client publishing Twists via zenoh, bridged to ROS2.

 1. Start the `turtlesim`:
    ```bash
    ros2 run turtlesim turtlesim_node
    ```
 2. Start the zenoh router:
    ```bash
    zenohd
    ```
 3. Start the zenoh/DDS bridge:
      ```bash
      dzd
      ```
 4. Start Ros2Teleop
      ```bash
      dotnet run -p Ros2Teleop.csproj
      ```
 5. Use the arrows keys to drive the robot

**Notes**:

By default Ros2Teleop publishes Twist messages on topic `/rt/turtle1/cmd_vel`. This can be changed using the `-tc` option:
  ```bash
  dotnet run -p Ros2Teleop.csproj -- -tc /rt/my_robot/cmd_vel
  ```

The both zenoh router and Teleop can be deployed in different networks than the robot. Only the zenoh/DDS bridge has to run in the same network than the robot (for DDS communication via UDP multicast).  
For instance, you can:
 * deploy the zenoh router in a cloud on a public IP with port 7447 open
 * configure the zenoh bridge to connect this remote zenoh router:
   ```bash
   dzd -m client -e tcp/<cloud_ip>:7447
   ```
 * configure Ros2Teleop to connect this remote zenoh router:
  ```bash
  dotnet run -p Ros2Teleop.csproj -- -m client -e tcp/<cloud_ip>:7447
  ```


---
### HelloWorldSubscriber / HelloWorldPublisher

Those are simple HelloWorld examples that interoperate with [CycloneDDS' HelloWorld](https://github.com/eclipse-cyclonedds/cyclonedds/tree/master/examples/helloworld)

#### zenoh-only usage (peer-to-peer):

 1. Start the subscriber:  
      ```bash
      dotnet run -p HelloWorldSubscriber.csproj
      ```
 2. Start the publisher:
      ```bash
      dotnet run -p HelloWorldPublisher.csproj
      ```

#### CycloneDDS interop:

 1. Start the zenoh router:
      ```bash
      zenohd
      ```
 2. Start the zenoh/DDS bridge:
      ```bash
      dzd
      ```
 3. For zenoh C# -> CycloneDDS
    * Start the CycloneDDS subscriber:
        ```bash
        HelloworldSubscriber
        ```
    * Start the zenoh C# publisher:
        ```bash
        dotnet run -p HelloWorldPublisher.csproj
        ```
 4. For CycloneDDS -> zenoh C#
    * Start the zenoh C# subscriber:
        ```bash
        dotnet run -p HelloworldSubscriber.csproj
        ```
    * Start the CycloneDDS publisher:
        ```bash
        HelloWorldPublisher
        ```
