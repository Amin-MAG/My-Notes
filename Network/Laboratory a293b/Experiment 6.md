# Experiment 6

## Overview

In some cases, I used this command to jump into the VM machine from another machine in my local network.

```bash
ssh mininet@192.168.1.10
```

### ❓ Question 1

Scenario I

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled.png)

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%201.png)

Host 1

- goodput: 1Mbps
- loss: %0

Host 2

- goodput: 1Mbps
- loss: %0

Scenario II

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%202.png)

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%203.png)

Host 1

- goodput: 935Kbps
- loss: %5.4

Host 2

- goodput: 1943Kbps
- loss: %2.8

Scenario III

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%204.png)

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%205.png)

Host 1

- goodput: 594Kbps
- loss: %49.5

Host 2

- goodput: 2329Kbps
- loss: %46.7

### ❓ Question 2

Based on the result, They’re much like each other. The difference is caused by approximate calculation. We consider the send rate permanent, and this is not a correct fact.

### ❓ Question 3

Scenario I

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%206.png)

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%207.png)

Host 1

- goodput: 890Kbps
- loss: %13.8

Host 2 - UDP

- goodput: 790Kbps
- loss: %23.2

Host 2 - TCP

- goodput: 1230Kbps
- loss: -

Scenario II

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%208.png)

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%209.png)

Host 1

- goodput: 775Kbps
- loss: %22.1

Host 2 - UDP

- goodput: 1650Kbps
- loss: %24.4

Host 2 - TCP

- goodput: 588Kbps
- loss: -

Scenario III

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%2010.png)

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%2011.png)

Host 1

- goodput: 536Kbps
- loss: %48.3

Host 2 - UDP

- goodput: 2300Kbps
- loss: %47.2

Host 2 - TCP

- goodput: 12.5Kbps
- loss: -

Just like previous question, They’re not much different. We should send with static rate and bandwidth.

### ❓ Question 4

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%2012.png)

Host 1

- Rate: 4580Kbps
- RTT: 3265ms
- Cwnd: [1355-2533]

### ❓ Question 5

![Untitled](Experiment%206%208daf9964cc7e4ae499028d9f895923c6/Untitled%2013.png)

Host 1

- Rate: 4671Kbps
- RTT: 47ms
- Cwnd: [13-21]

### ❓ Question 6

Values of RTT decreases when ECN is active. The sending rate for both of the are same. Router queue will overflow when ECN in inactive and that’s why time of queueing and RTT is going to increase.