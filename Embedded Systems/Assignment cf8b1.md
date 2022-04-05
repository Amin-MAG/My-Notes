# Assignment I

The assignment is about the Smart Car Control Systems. The car should reach the destination from a starting point.

- Calculates the path to the destination: We need to calculate the current location and then send it to the cloud to find out the best path to the desired destination.
- Changes the velocity of the car based on the traffic volume and permitted speed: ACC will control this based on the information coming from the cloud.
- Traffic Volume: low, relatively low, relatively high, or high: Cloud should implement an algorithm to estimate the traffic volume or It can use a third-party SDK. (eg. Google Map)
- Low Traffic Volume
    - Rides at the most left line of the road
    - Rides at the highest possible velocity
- Relatively Low Traffic Volume
    - It should have a 10 meters distance from the next car to ride at the highest possible speed. (A connected smart camera captures the distance)
    - Ride at the right side of the most left line of the road.
- High & Relatively High Traffic Volume
    - At most, It should have a 3 meters distance from the next car. It rides at the highest possible speed.
    - Rides at the most left line of the road
- Traffic light recognition: The front camera should process the data and recognize green/red traffic lights.
- Take care of pedestrians by using a camera: The front camera should process the data and recognize pedestrians.

## Components of the system

The main component of our systems is ACC or adaptive cruise controller. It automatically controls the acceleration and braking of a vehicle. There are different types of ACC such as Rader-based ACC, Laser-based ACC, or...

Two of the most important pieces used for ACC are usually

- A `Millimeter-wave radar` in front of the car: It’s good for measuring the distance of vehicles ahead and their change in speed. (wavelength in Microwave wave region)
- A `camera` in front of the car: It’s a much better object recognition tool although it is not a good judging distance tool. (wavelength in Visible light wave region)
- An actuator that controls the speed of the car

Now that we can recognize the distance and objects ahead of the car we need to have our current car speed. A `Hall Effect Sensor` can calculate the speed of our car.

Calculating the path to the destinations needs communication to the server so we need a `GSM module` to connect the system to the network.

We are going to estimate the system cost and I ignored the price of buying a sim card and the traffic that is going to be used (Actually it could be important in some cases, for example, if we want to stream the camera data to the cloud)

We also need to send our current location (latitude and longitude) so that a `GPS module` is required. There are different variants of GPS modules.

I think it’s better to use raspberry pi here; we have lots of image processing in our system (Although we need to offload some of these data to be processed in the edge or cloud). It has a wireless adapter we can switch our network connection as soon as possible. Also, It is a full computer, and we can connect our modules easier.

## Estimation of the cost of required components

Here I estimate the main components of this system.

- AWR1642 77GHz to 79GHz Automotive mmWave Sensor costs about `€31,14`
- An 8-Mega Pixel camera `1,360,000 Toman`
- MH182MST Hall Effect Sensor is about `€0.36`
- GSM Module `$11.9`
- u-blox 6 GPS module ROM, TCXO, 1.8V `€28.47`
- Raspberry Pi 4 B 8 GB 4 `€80.47`
- An actuator is needed for changing the velocity of the car it’s between `$250-1000` based on the quality you want to have.

## Digital Components

Our processor is a digital component. Memories and busses used on the raspberry pi are also Digital.

## Analog Components

Most of the sensors that we have in this system are analog components. Also, we have some actuators that are analog too.

## Being an Embedded System

Embedded systems are usually such devices other than PCs, servers, and notebooks that perform something intelligent & electricity running through them.

Of course, we have electricity in our system and we try to have an intelligent car.

Plus, our system has some of the characteristics of embedded systems:

- Being Real-time (described in the next section)
- Having Energy efficiency: because of using a mini-computer like raspberry pi
- Being Reactive: Our system continuously receives metrics and data from the sensors and tries to change the environment like the speed of the car or the direction of the car by actuators.
- Being Hybrid (described in the following)

## Being a Real-Time System

The correct answers for real-time systems are not enough. The correct answers calculated at the wrong time are wrong. Our system is a real-time system; we need the correct answers and decisions at the right time. The car speed could be between `0km/h - 120km/h`, which means that any second plays a role in this system. Having delays to respond can cause accidents and significant troubles for the driver.

Also, consider Most real-time systems are embedded and most of the embedded systems are real-time.

![Untitled](Assignment%20cf8b1/Untitled.png)

## Being a Hybrid System

This system consists of digital and analog components, then this system is a Hybrid System.

## What should we take care of about design and implementation?

Reactive-System: As I mentioned above, we should try to have an excellent real-time system with the lowest possible delay. The system must react instantly to the incidents.

Error handling: In my opinion, Error handling is another important aspect of our system.

- What if we lose our internet connection?
- What if the cloud can not calculate the permitted speed in the current location?
- What if the camera goes off?
- And...

Having an excellent error-handled system for this scenario is necessary because it will deal with people's lives.

Financial: It has much complexity to implement the ACC (Adaptive Cruise Controller) ourselves. We should decide between buying the whole ACC module or implementing it.

Fault tolerance: We use object recognition to act more intelligently in our system. But, image processing may have a kind of `%0.001` error. So It's better to use a mixture of millimeter-wave radar and camera to have better fault tolerance.

## Steps of designing and implementing the system.

The main steps are:

- Writing a requirement document → to get the concept of the system
- Specification that includes Hardware and Software Components
- Software and Hardware Partitioning
    1. What is the cost of this design?
    2. How fast is the system performance?
    3. What are the dimension of the systems?
    4. How they use energy?
- Choosing a hardware and a software design and implementing the prototype
- Compile the software code and load it into hardware

### Processors

Since we need to capture data from the camera and process them, Maybe Having a General-Purpose processor for this kind of system is not enough. We also need to have instant responses and decisions. So using a high-performance module is better. (e.g., ASIPs or ASICs processors)

### Memory

In this system we don’t want to store data. The requirement is to have high end write-ability. Using SRAM, DRAM is better here.

# References

[](https://www.researchgate.net/figure/Smart-Car-System-17_fig1_325130178)

[What is Adaptive Cruise Control? | TomTom Blog](https://www.tomtom.com/blog/automated-driving/what-is-adaptive-cruise-control/)

[Adaptive cruise control - Wikipedia](https://en.wikipedia.org/wiki/Adaptive_cruise_control)

[What Is Adaptive Cruise Control?](https://www.caranddriver.com/research/a32813983/adaptive-cruise-control/)

Stores links

[RF Systeem op een Chip - SoC - Mouser Nederland](https://nl.mouser.com/c/?marcom=154139575)

[دوربین 8 مگاپیکسل رزبری پای V2.1 با سنسور IMX219](https://daneshjookit.com/board/raspberry-pi/%D8%B1%D8%B2%D8%A8%D8%B1%DB%8C-%D9%BE%D8%A7%DB%8C-raspberry-pi/1891-raspberry-pi-no-ir-original-camera.html)

[Mh182mst Hall Effect Sensors](https://www.indiamart.com/proddetail/mh182mst-hall-effect-sensors-11681624648.html?pos=1&pla=n)

[USB to GSM Serial GPRS SIM800C Module With Bluetooth Antenna Computer Control | eBay](https://www.ebay.com/itm/144123357946?hash=item218e6beafa:g:XlgAAOSwqOFb4bvL)

[NEO-6 series](https://www.u-blox.com/en/product/neo-6-series)