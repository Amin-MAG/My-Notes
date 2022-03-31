# Performance Testing

Performance testing aims to examine system behavior and performance. Specifically, it monitors the response time, scalability, speed, and resource utilization of the software and infrastructure.

## Stress Testing

`empty_now`

## Load Testing

Load testing generally refers to **the practice of modeling the expected usage of a software program by simulating multiple users accessing the program concurrently**. As such, this testing is most relevant for multi-user systems; often one built using a client/server model, such as web servers.

### How it works

It measures the speed or capacity of the system or component through transaction response time. When the system components dramatically extend response times or become unstable, the system is likely to have reached its maximum operating capacity. When this happens, the bottlenecks should be identified and solutions provided.

### Load Testing vs Stress Testing

There is not much **[difference between load testing and stress testing](https://stackify.com/load-testing-vs-performance-testing-vs-stress-testing/)**, which is the reason why they are often confused with each other. Load testing and stress testing are both subsets of performance testing.

Load testing checks how the systems behave under normal or peak load conditions. Stress testing, on the other hand, is applied to check how the system behaves beyond normal or peak load conditions and how it responds when returning to normal loads.

So, Load testing identifies the bottlenecks in the system under various workloads and checks how the system reacts when the load is gradually increased. [Stress Testing](https://www.guru99.com/stress-testing-tutorial.html) determines the breaking point of the system to reveal the maximum point after which it breaks.

### Strategies

- **Manual Load Testing**: This is one of the strategies to execute load testing, but it does not produce repeatable results, cannot provide measurable levels of stress on an application and is an impossible process to coordinate.
- **In house developed load testing tools**: An organization, which realizes the importance of load testing, may build their own tools to execute load tests.
- **Open source load testing tools**: There are several load testing tools available as open source that are free of charge. They may not be as sophisticated as their paid counterparts, but if you are on a budget, they are the best choice.
- **Enterprise-class load testing tools**: They usually come with capture/playback facility. They support a large number of protocols. They can simulate an exceptionally large number of users.

### Process to load test

1. Create a dedicated [Test Environment](https://www.guru99.com/test-environment-software-testing.html) for load testing
2. Determine the following
3. Load Test Scenarios
4. Determine load testing transactions for an application
    - Prepare Data for each transaction
    - Number of Users accessing the system need to be predicted
    - Determine connection speeds. Some users may be connected via leased lines while others may use dial-up
    - Determine different browsers and operating systems used by the users
    - A configuration of all the servers like web, application and DB Servers
5. Test Scenario execution and monitoring. Collecting various metrics
6. Analyze the results. Make recommendations
7. Fine-tune the System
8. Re-test

### Benefits

Benefits include the discovery of bottlenecks before production, scalability, reduction of system downtime, improved customer satisfaction, and reduced failure costs.

- **Discovering bottlenecks before deployment.**
- **Enhance the scalability of a system.**
- **Reduced risk for system downtime.**
- **Improved customer satisfaction.**
- **Reduced failure cost**

### Best Practices

- **Identify business goals.**
- **Determine key measures for the application and web performance.**
- **Choose a suitable tool.**
- **Create a test case.**
- **Understand your environment.**
- **Run tests incrementally.**
- **Always keep end-users in mind.**

# Read More
- [Locust](Locust.md)
- [K6](K6.md)

# References

- [What is Load Testing? How It Works, Tools, Tutorials, and More](https://stackify.com/what-is-load-testing/)
- [Load Testing Tutorial: What is? How to? (with Examples)](https://www.guru99.com/load-testing-tutorial.html#7)

