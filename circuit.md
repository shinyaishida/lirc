Circuits
====

## IR transmitter

IR transmitter driven through a GPIO pin of Raspberry Pi (3 Model B)

### Circuit diagram

```
5.0V o-------+-------+
             |       |
            [R1]     |
             |       S
             +---G[P-FET]
             |       D
             D       |
  IN o---G[N-FET]    +-------+----+
             S               |    |
             |              [R2] [R2]
             |               |    |
 GND o-------+---[<IR-LED]---+----+

R1:     4.7kohm, 1/2W
R2:     15ohm, 1W
N-FET:  2N7000
P-FET:  IRFU9024NPBF
IR-LED: OSI5LA5113A
```

This circuit has three terminals to connect to 5V, GND, and a 3.3V input signal pin, respectively. The input signal drives the N-channel FET ([`SN7000`](http://akizukidenshi.com/download/2n7000.pdf)), which drives the gate voltage of the P-channel FET ([`IRFU9024NPBF`](http://akizukidenshi.com/download/ds/ir/IRFU9024NPBF_IRFR9024NPBF.pdf)), i.e., if the gate voltage of the N-channel FET is low/high, the gate voltage of the P-channel FET is high/low. If the gate voltage of the N-channel FET is low, an electric current flows from the source to the drain. Then, the Infrared LED ([`OSI5LA5113A`](http://akizukidenshi.com/download/ds/optosupply/OSI5LA5113A.pdf)) is lit up.

Transmission disntace of infrared signal from IR-LED is extended by increasing the amount of electric current. [This circuit may momentarily flow a ~500mA electric current through the IR-LED](https://vintagechips.wordpress.com/2013/10/05/%e8%b5%a4%e5%a4%96%e7%b7%9aled%e3%83%89%e3%83%a9%e3%82%a4%e3%83%96%e5%9b%9e%e8%b7%af%e3%81%ae%e6%b1%ba%e5%ae%9a%e7%89%88/). The typical forwarding voltage of `OSI5LA5113A` is 1.35V. Therefore, a 7.5ohm resistor is inserted between the IR-LED and P-channel MOSFET. Note that the resistor is comprised of two 15ohm parallel resistors whose power rating is 1W.

### Implementation on breadboard

```
  5.0V                 IN GND
A  o      [S N-FET G]  o   o       +
   |       |   |   |   |   |       |
B  o----[ R1  ]----o   +   o--[<]--o
   |       |   |   |   |   |       |
C  +       +   o------[ R2  ]------o
   |       |   |   |   |   |       |
D  +       +   o------[ R2  ]------o
   |       |   |   |   |   |       |
E  o-------o   +  [D P-FET S]      +

   1   2   3   4   5   6   7   8   9
```

## IR receiver

IR receiver to relay signals to a GPIO pin of Raspberry Pi (3 Model B)

### Circuit diagram

```
3.3V o---[R3]---+------+
                |      |
 OUT o-------[IR-RCV] [C1]
                |      |
 GND o----------+------+

R3:     100ohm, 1/2W
C1:     47uF, 16V
IR-RCV: OSRB38C9AA
```


[`OSRB38C9AA`](http://akizukidenshi.com/download/OSRB38C9AA.pdf)

### Implementation on breadboard

```
  3.3V                GND OUT
A  o           +   +   o   o
   |           |   |   |   |
B  +           +  [3   2   1] <-- IR-RCV (facing upward)
   |           |   |   |   |
C  o----[ R3  ]----o   +   +
   |           |   |   |   |
D  +           o---o   +   +
   |           |   |   |   |
E  +           o--[C1]-o   +

   1   2   3   4   5   6   7
```
