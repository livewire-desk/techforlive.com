---
title: Bluetooth car with Arduino and HC-05: a first build that drives
seo_title: Build a Bluetooth car with Arduino and HC-05 now
description: Wire an HC-05 to an Arduino Uno, drive two motors through an L298N, and steer the car from your phone with one-letter commands.
date: 2026-11-16T13:04:00
updated: 2026-11-16T13:04:00
category: robotics-diy
desk: science
image: https://tse4.mm.bing.net/th?q=bluetooth+controlled+car+with+arduino+and+hc-05&w=820&h=462&c=7&rs=1&p=0&o=5&pid=1.7
image_alt: Small robot car with Arduino Uno, HC-05 Bluetooth module and L298N driver board
source: contentforge
cf_id: 15017
---
<div class="key"><b>In short:</b> A Bluetooth controlled car with Arduino and HC-05 links your phone to a small robot car. The HC-05 passes one-letter drive commands to an Arduino Uno. The Uno tells an L298N driver board how fast each wheel turns, and in which direction. You build it with a few common parts and one sketch.<ul><li>Wire the HC-05 RX pin through a 1k and 2k resistor divider, or 5V will reach a 3.3V pin.</li><li>Give the motors their own battery and join the grounds, or the Arduino resets when the wheels start.</li><li>Use single letters for commands: F, B, L, R and S keep the phone app and the sketch simple.</li><li>Pair with PIN 1234 first; some modules ship with 0000 instead.</li></ul></div>
<h2>How a Bluetooth controlled car with Arduino and HC-05 works</h2>
<p>Your phone sends a single letter over Bluetooth each time you tap a button. That letter reaches the HC-05 module, which hands it to the Arduino over a serial link. From there the Arduino sets the motor driver pins for forward, back, left, right or stop. Nothing here needs the internet. It is a short radio hop between two boards on the same desk.</p>
<p>Three boards do three jobs. One board handles the radio link: the HC-05 speaks serial, which is a stream of bytes over two wires. Your sketch runs on the Arduino Uno, which decides what each letter means. Real motor current comes from the L298N driver board, which switches far more than Arduino pins can supply on their own. Each board is cheap and easy to replace. That split keeps the build simple to test in stages.</p>
<p>Steering works by driving the two wheels at different speeds. Forward means both wheels turn the same way. A left turn slows or stops the left wheel while the right wheel keeps turning. It pivots around the slow wheel. This is called differential drive, and it needs no steering servo. Two motors and one driver board are the whole steering system.</p>
<table><thead><tr><th>At a glance</th><th>Answer</th></tr></thead><tbody><tr><td>HC-05 data baud</td><td>9600 baud</td></tr><tr><td>HC-05 AT-mode baud</td><td>38400 baud</td></tr><tr><td>HC-05 logic level</td><td>3.3 V</td></tr><tr><td>Divider resistors</td><td>1k and 2k ohms</td></tr><tr><td>Bluetooth range</td><td>a few metres indoors</td></tr><tr><td>Drive commands</td><td>F, B, L, R, S</td></tr></tbody></table>
[[tool:battery-life-calculator]]
<h2>The parts and the wiring map</h2>
<p>You need an Arduino Uno, an HC-05 Bluetooth module and an L298N driver board. Add two small DC gear motors with wheels, a chassis or a stiff board, jumper wires, a battery pack and two resistors. Most sellers bundle the motors, wheels and chassis as one robot car kit. Buy the kit if you can. It saves a fiddly afternoon of drilling and gluing.</p>
<p>Batteries matter more than most first builds admit. A single 9V block looks tidy, but it sags the moment both motors start and the Arduino may reset. Six AA cells in a holder give far more current and last longer. Two rechargeable 18650 cells in a holder work too, at about 7.4V. Handle 18650 cells with care: use a proper holder and a charger made for them, and never short the ends. Pick one pack and use it only for the motors. While you test, the Arduino runs from USB, then from its own supply later.</p>
<p>Hold the wiring map in front of you while you connect things. Here the HC-05 talks to Arduino pins 10 and 11 through a software serial port. Six Arduino pins go to the L298N: two enable pins for speed and four direction pins. Motor power comes from the battery, which feeds the L298N motor terminals, never the Arduino 5V pin. That table is the whole map, so check each wire against it before you switch anything on.</p>
<table><thead><tr><th>Arduino pin</th><th>Connects to</th><th>Note</th></tr></thead><tbody><tr><td>10</td><td>HC-05 TXD</td><td>Arduino receives data</td></tr><tr><td>11</td><td>HC-05 RXD</td><td>through the 1k and 2k divider</td></tr><tr><td>5 (PWM)</td><td>L298N ENA</td><td>left motor speed</td></tr><tr><td>6 (PWM)</td><td>L298N ENB</td><td>right motor speed</td></tr><tr><td>7</td><td>L298N IN1</td><td>left motor direction</td></tr><tr><td>8</td><td>L298N IN2</td><td>left motor direction</td></tr><tr><td>4</td><td>L298N IN3</td><td>right motor direction</td></tr><tr><td>2</td><td>L298N IN4</td><td>right motor direction</td></tr><tr><td>GND</td><td>L298N GND and battery minus</td><td>common ground</td></tr><tr><td>5V</td><td>HC-05 VCC</td><td>module power</td></tr></tbody></table>
<h2>Wire the HC-05 so 5V never reaches its RX pin</h2>
<figure class="fig"><img src="https://tse1.mm.bing.net/th?q=HC-05+module+wired+to+Arduino+with+two+resistors+on+a+breadboard&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="HC-05 module wired to Arduino with two resistors on a breadboard" loading="lazy" decoding="async"></figure>
<p>Inside the HC-05, everything runs on 3.3V logic. Its VCC pin accepts 5V from the Arduino, because most breakout boards carry their own regulator. Its RX pin does not. Arduino TX sends 5V, and feeding that straight into the HC-05 RX pin can damage the module over time. A voltage divider made from two resistors fixes it. It costs cents and takes a minute.</p>
<p>Join a 1k resistor between Arduino pin 11 and the HC-05 RX pin. Join a 2k resistor between the HC-05 RX pin and ground. Standard quarter-watt resistors are fine for this divider. Together they split the 5V, and the HC-05 RX pin sees about 3.3V. That is the divider the Arduino forum and module guides recommend. No divider is needed on the HC-05 TX pin, because its 3.3V output still reads as HIGH on a 5V Arduino input. Connect it straight to Arduino pin 10.</p>
<p>Power the module from the Arduino 5V pin and ground. HC-05 modules use an LED to show status. Typical patterns are a fast blink when discoverable and unpaired, and a steadier or slower blink when paired or in certain modes. Exact patterns vary by module, so check the datasheet or vendor notes for your board. Check the LED before you blame the code. If the LED never lights, check VCC and ground first. If it blinks but the phone cannot see it, the module may already be paired to another phone.</p>
<div class="note warn"><b>Warning:</b> Never connect the Arduino TX pin straight to the HC-05 RX pin. The 5V signal can damage a 3.3V input, and the failure shows up weeks later as a dead module.</div>
[[amazon:HC-05 Bluetooth module Arduino|HC-05 Bluetooth module|The radio link for the car; any HC-05 breakout board with 3.3V logic works.]]
[[amazon:L298N motor driver module Arduino|L298N driver board|Drives both motors from one board and takes the motor current the Arduino cannot supply.]]
<h2>Wire the L298N and give the motors their own battery</h2>
<p>Between the Arduino and the motors sits the L298N. It takes small 5V logic signals from the Arduino and switches the bigger motor current from the battery. One board drives both wheels, because it holds two H-bridges, one per motor. An H-bridge is a set of switches that flips which way current flows through a motor. Flip the switches one way and the wheel turns forward. Flip them the other way and it turns back.</p>
<p>Connect the battery plus lead to the L298N terminal marked 12V and the minus lead to GND. Connect the Arduino GND pin to the same L298N GND terminal. This shared ground is the wire most first builds forget, and without it the direction signals mean nothing to the driver board. Without it, the motors behave at random or sit still. Check that wire before you test anything else.</p>
<p>Each motor connects across one output pair: OUT1 and OUT2 for the left motor, OUT3 and OUT4 for the right. Remove the small jumper caps on ENA and ENB if you want speed control, then wire those pins to Arduino PWM pins 5 and 6. Leave the 5V-enable jumper in place when your motor battery is 12V or less, so the board makes its own 5V for the logic side. Inside the chip, the L298N drops about 2V, so a 7.4V pack gives the motors roughly 5V. That is normal for this board. Most L298N boards already carry flyback diodes, so you usually add none for the motors.</p>
<div class="note warn"><b>Common mistake:</b> Never power the motors from the Arduino 5V pin. The motors pull far more current than the pin can give, and the board will reset or brown out.</div>
<h2>The sketch that turns letters into motion</h2>
<figure class="fig"><img src="https://tse4.mm.bing.net/th?q=Arduino+IDE+on+a+laptop+screen+showing+robot+car+code&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="Arduino IDE on a laptop screen showing robot car code" loading="lazy" decoding="async"></figure>
<p>Keep the sketch short on purpose. It opens a software serial port on pins 10 and 11 for the HC-05, so the USB serial port stays free for the Serial Monitor. It sets the six L298N pins as outputs. Then it loops, reading one letter at a time from the Bluetooth link. Each letter runs a small function: forward, back, left, right or stop. Copy the sketch below as shown: it is complete and it compiles.</p>
<p>Speed comes from analogWrite on the enable pins, with 0 for stopped and 255 for full speed. A value of 200 keeps it brisk without being wild on a desk. Lower it to 150 if the car jumps when it starts. Direction comes from the IN pins: one HIGH and one LOW turns a motor one way, and the pair reversed turns it the other way. Both LOW stops that motor. A left turn stops the left motor and runs the right one, so the car pivots.</p>
<p>Upload the sketch over USB as normal. Pins 10 and 11 carry the HC-05 link, so it never fights the USB upload on pins 0 and 1. Open the Serial Monitor at 9600 baud and type F, then press send. It should roll forward. If nothing moves, check the wiring table again before you change the code. Most first failures are a wire, not a sketch.</p>
<p><b>Bluetooth car sketch for Arduino Uno</b></p><pre><code>#include &lt;SoftwareSerial.h&gt;
SoftwareSerial bt(10, 11); // RX, TX

const int ENA = 5, IN1 = 7, IN2 = 8;   // left motor
const int ENB = 6, IN3 = 4, IN4 = 2;   // right motor
const int SPEED = 200;

void setup() {
  pinMode(ENA, OUTPUT); pinMode(IN1, OUTPUT); pinMode(IN2, OUTPUT);
  pinMode(ENB, OUTPUT); pinMode(IN3, OUTPUT); pinMode(IN4, OUTPUT);
  bt.begin(9600);
}

void loop() {
  if (bt.available()) {
    char c = bt.read();
    if (c == &#x27;F&#x27;) drive(1, 1);
    else if (c == &#x27;B&#x27;) drive(-1, -1);
    else if (c == &#x27;L&#x27;) drive(0, 1);
    else if (c == &#x27;R&#x27;) drive(1, 0);
    else if (c == &#x27;S&#x27;) drive(0, 0);
  }
}

void drive(int left, int right) {
  setMotor(ENA, IN1, IN2, left);
  setMotor(ENB, IN3, IN4, right);
}

void setMotor(int en, int in1, int in2, int dir) {
  if (dir &gt; 0) { digitalWrite(in1, HIGH); digitalWrite(in2, LOW); }
  else if (dir &lt; 0) { digitalWrite(in1, LOW); digitalWrite(in2, HIGH); }
  else { digitalWrite(in1, LOW); digitalWrite(in2, LOW); }
  analogWrite(en, dir == 0 ? 0 : SPEED);
}</code></pre>
<h2>Drive it from your phone and fix what fails</h2>
<p>Pair the phone to the HC-05 from the phone Bluetooth settings, not from inside the app. It shows up as HC-05, and the PIN is usually 1234. Some modules ship with 0000, so try that next if 1234 fails. Open a Bluetooth RC car app once the phone says paired. Apps such as Arduino Bluetooth RC Car send one letter per button press, which is exactly what the sketch expects. Tap the forward arrow and the car should roll.</p>
<p>A car that spins in place has one motor wired backwards. Swap the two wires on one motor at the L298N terminals and test again. Crawling points to a tired battery pack, so fit a fresh one. No response to the phone means the HC-05 TX and RX lines may be crossed. TXD goes to Arduino pin 10 and RXD goes to pin 11 through the divider. Swap them only if the module never answers.</p>
<p>Range is the last limit to accept. Typical indoor usable range for the HC-05, a Bluetooth Classic class 2 module, is a few metres to a dozen metres. Walls and interference reduce that. Walk to the next room and the car will lag or stop. That is normal for this module, not a fault in your build. Keep the phone in the same room as the car. If you need more range, test your own module or choose a different radio.</p>
<h2>Quick answers</h2>
<h3>Can I use an HC-06 instead of an HC-05?</h3>
<p>Yes, for this build. As a slave-only module, the HC-06 still works here, because this car only needs to listen. Wiring and sketch stay unchanged. Buy the HC-05 only if you later want the module to start connections itself.</p>
<h3>Why does my car reset when the motors start?</h3>
<p>At start-up, the motors pull a surge of current, and a weak battery sags below the level the Arduino needs. Give the motors their own fresh pack and join the grounds. Keep the Arduino on USB or its own supply while you test.</p>
<h3>Do I need the voltage divider if my HC-05 board has a regulator?</h3>
<p>Yes, usually. On most breakout boards, the regulator handles the power pin, not the RX pin. That RX pin still expects 3.3V logic, so keep the 1k and 2k divider in place. A level shifter works too, and it is the tidier option if you plan to reuse the module.</p>
<h3>Can I control the car from a laptop instead of a phone?</h3>
<p>Yes. Pair the laptop to the HC-05, open a serial terminal at 9600 baud, and type the same letters. It cannot tell a laptop from a phone, because both send the same single letters. A laptop is handy for testing, and the phone is better for driving.</p>
<h3>How far will the car drive from my phone?</h3>
<p>A few metres to a dozen metres indoors, which is the normal range for a Bluetooth Classic module like the HC-05. Walls and other 2.4 GHz devices cut that down. Drive in the same room, and expect the link to drop if you walk away with the phone.</p>
