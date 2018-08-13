# [How The Internet Works ?](https://www.youtube.com/playlist?list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)

In the early 70s, **Vint Cerf** and **Bob Kahn** work on the design of what we called the Internet.

It was a result of another experiment called the **ARPANET** *which stood for **A**dvanced **R**esearch **P**roject **A**gency **N**etwork*.

The **Internet** is made up of an incredibly large number of independently operated networks. 

The utility of the net is that any device can communicate with any other device and move information.

> What's interesting about the system is that it's fully distributed. There's no central control that is deciding how packets are routed or where pieces of the network are built or even who interconnects with whom. These are all business decisions that are made independently by the operators.

Thereby, **anybody** and **everybody** are in charge of the Internet.

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

#### *... how can we send five zeros in a row ?*

The solution is to introduce a clock or a timer. 

The operators can agree that the sender will send one bit per second, the receiver will sit down and record every single second and see what's on the line.

Obviously we'd like to send things a little bit faster than one bit per second so we need to increase our **bandwidth**.

**Bandwidth** : maximum transmission capacity of a device.

**Bandwidth** is measured by **bit rate**.

**Bit rate** : number of bits that we can send over a given period time *usually measured in seconds*.

**Latency** (*different measure of speed is the latency*) : times it takes for a bit to travel from one place to another.

#### *... what sort of cable to send these messages over ? and how the signals can go ?*

With an **Ethernet wire**, you see really measurable signal loss over just a few hundred feet.

If we want the Internet to work over the entire world, we need a different way of sending this information really long distances (*across the oceans for instance*). 

**Light** move faster than **Electricity**.

A **Fiber optic cable** is a thread of glass engineered to reflect light.

With a **Fiber optic cable**, we can send bits as light beams from one place o another.

