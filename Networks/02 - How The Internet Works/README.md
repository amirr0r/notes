# [How The Internet Works ?](https://www.youtube.com/playlist?list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)

In the early 70s, **Vint Cerf** and **Bob Kahn** work on the design of what we called the Internet.

It was a result of another experiment called the **ARPANET** *which stood for **A**dvanced **R**esearch **P**roject **A**gency **N**etwork*.

The **Internet** is made up of an incredibly large number of independently operated networks. 

The utility of the net is that any device can communicate with any other device and move information.

> What's interesting about the system is that it's fully distributed. There's no central control that is deciding how packets are routed or where pieces of the network are built or even who interconnects with whom. These are all business decisions that are made independently by the operators.

In conclusion, **the Internet is a networks of networks** and ehereby **anybody** and **everybody** are in charge of it.
___

## [Wires, Cables & Wifi](https://www.youtube.com/watch?v=ZhEf7e4kopM&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7&index=2)

### How does a picture, text message or email gets sent from one place to another ?

The **Internet** is a lot like the **postal service** !

However, instead of boxes and envelopes the Internet **ships binary information**.

**Information is made of bits** *like we saw in* [How Computers works ?](https://github.com/kecro/notes/tree/master/Networks/01%20-%20How%20Computers%20Works#how-computers-work-).

```
8 bits = 1 byte
1024 bytes = 1 Ko (kilobyte)
1024 kilobytes = 1 Mo (megabyte)
```

Today we **physically send bits** by **electricty, light and radio waves**.

___

#### *... how can we send five zeros in a row ?*

The solution is to introduce a clock or a timer. 

The operators can agree that the sender will send one bit per second, the receiver will sit down and record every single second and see what's on the line.

Obviously we'd like to send things a little bit faster than one bit per second so we need to increase our **bandwidth**.

**Bandwidth** : maximum transmission capacity of a device.

**Bandwidth** is measured by **bit rate**.

**Bit rate** : number of bits that we can send over a given period time *usually measured in seconds*.

**Latency** (*different measure of speed is the latency*) : times it takes for a bit to travel from one place to another.

___

#### *... what sort of cable to send these messages over ? and how the signals can go ?*

With an **Ethernet wire**, you see really measurable signal loss over just a few hundred feet.

If we want the Internet to work over the entire world, we need a different way of sending this information really long distances (*across the oceans for instance*). 

**Light** move faster than **Electricity**.

A **Fiber optic cable** is a thread of glass engineered to reflect light.

With a **Fiber optic cable**, we can send bits as light beams from one place o another.

___

#### *... how do we send things wirelessly ?*

**Wireless bits sending machines** typically use a radio signal to send bits from one place to another.

The machine **translate** the **ones and zeros** into **radio waves** of different frequencies.

The receiving machines reverse the process and convert it back into binary code.
___

## [IP Addresses & DNS](https://www.youtube.com/watch?v=5o8CwafCxnU&index=3&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)

The Internet is a design philosophy and a architecture expressed in a set of **protocols**.

**Protocol** : set of rules and standards used to communicate between machines.

All the different devices on ther Internet have **unique addresses** which are just numbers like phone numbers.

One of the most important protocols used in Internet communication simply call **IP** (**I**nternet **P**rotocol).

Just like a home address has a country, a city, a street, and a house number, an **IP adress** has many parts.

- **Earlier numbers** usually **identify** the **country** and **regional** network of the device.
- Then come the **subnetworks**.
- And finally the adress of the **specific device**.

This is the **IPv4** version, designed in **1973** and adopted in the **1980**'s and provides more than 4 billion unique adresses.

**IPv6** has recently appeared to fill the void of available addresses. It uses **128 bits**/**16 bytes** per address and provides over **340 undecillion unique addresses**.

> 3,4 x 10^38 équivaut à un nombre illimité puisque pour saturer le système, il faudrait placer plus de 667 millions de milliards d'appareils connectés à internet sur chaque millimètre carré de surface terrestre.

___

A system called **DNS** or **D**omain **N**ame **S**ystem associates names like [www.example.com](http://www.example.com) with the corresponding addresses.

You must know that there is no way one DNS Server can handle all the requests from all devices. 

DNS Servers are connected in a distributed hierarchy and are divided into zones, splitting up reponsabilities for the major domaines such as **.org**, **.com**, **.net** etc.

**DNS Spoofing** is an attack when a "*hacker*" taps into a DNS Server and changes it to match a domain name with the wrong IP address.
___

## [Packets, Routing & Reliability](https://www.youtube.com/watch?v=AYdF7b3nMto&index=4&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)

If the Internet were made of direct, dedicated connections, it would be impossible to keep things working as millions of users join.

>Especially since there is no guarantee that every wire and computer is working all the time.

**Data travels on the Internet in a much less direct fashion**.

Example : Airplane / Image + roads (traffics)

Special computers called **Routers** act like traffic managers to keep packets moving through the networks smoothly.

**More routers** we add, the **more  reliable** the Internet becomes.
___

## [HTTP & HTML](https://www.youtube.com/watch?v=kBXQZMmiA4s&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7&index=5)


