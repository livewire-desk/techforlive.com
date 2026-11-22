---
title: HC-05 car control: Arduino pins and phone commands
seo_title: HC-05 car control: Arduino pins and phone commands
description: Build a bluetooth controlled car with Arduino and HC-05: exact pins, divider, L298N wiring, phone commands and a safe first test.
date: 2026-11-23T02:02:00
updated: 2026-11-23T02:02:00
category: robotics-diy
desk: science
image: /media/arduino-uno-board-wired-to-a-small-sensor-module-w-d4b35866e2-1200.webp
image_alt: Arduino Uno board wired to a small sensor module with USB cable
source: contentforge
cf_id: 10797
---
<div class="key"><b>In short:</b> A bluetooth controlled car with Arduino and HC-05 needs crossed serial wires. It also needs a divider on the module receive line. An L298N sits between board and motors, and your phone sends single letters to steer.<ul><li>Cross the serial lines every time: module transmit goes to board receive, never transmit to transmit.</li><li>Treat pins <code>0</code> and <code>1</code> as shared with USB uploads, so expect to unplug a wire while you upload.</li><li>Give the motors their own supply and join the grounds, or the board will reset under load.</li><li>Add a stop letter and use it on release, or the car keeps its last order until it hits something.</li></ul></div>
<h2>Pins first: how the module talks to your board</h2>
<p>Your bluetooth controlled car with Arduino and HC-05 lives or dies on four wires. Get those right and the rest is code. Get one crossed and the phone pairs happily while nothing moves.</p>
<p>Most HC-05 breakout boards accept <strong>3.6 to 6 V</strong> on VCC and regulate it down inside. Underneath, the bare module runs on <strong>3.3 V</strong> logic. Feed the breakout from the Arduino <code>5V</code> pin and join both grounds together.</p>
<p>Serial data needs a cross. Module TXD goes to Arduino pin <code>0</code>, the receive pin. Arduino pin <code>1</code>, the transmit pin, goes back to module RXD. TX talks. RX listens. Two talkers on one wire hear nothing.</p>
<p>That return wire needs a voltage divider. An Uno sends <strong>5 V</strong> logic, while RXD expects <strong>3.3 V</strong> logic. A <code>1 kΩ</code> resistor from pin <code>1</code> and a <code>2 kΩ</code> resistor to ground drop the signal safely.</p>
<p>Leave EN and STATE unconnected for normal driving. EN only matters for AT mode, the setup mode for changing the name, code or speed.</p>
<p>Pins <code>0</code> and <code>1</code> also carry USB uploads on an Uno. Leave a module wired there and new sketches can fail. Unplug the wire on pin <code>0</code> while you upload, then reconnect it to drive.</p>
<ul><li>Label check: boards print RXD and TXD or plain RX and TX, so match letters, not positions</li><li>Divider swap: a logic level shifter module does the same job for readers without those resistor values</li><li>AT mode entry: hold EN high while power is applied, then talk at 38400 baud</li><li>Bench parts: Uno, HC-05 breakout, jumper wires and the two divider resistors</li></ul>
<table><caption>HC-05 to Arduino wiring list</caption><thead><tr><th>Module pin</th><th>Arduino side</th><th>Note</th></tr></thead><tbody><tr><td>VCC</td><td>5V pin</td><td>Breakout supply input</td></tr><tr><td>GND</td><td>GND pin</td><td>Shared ground</td></tr><tr><td>TXD</td><td>Pin 0 (RX)</td><td>Module talks, board listens</td></tr><tr><td>RXD</td><td>Pin 1 (TX)</td><td>Through the divider</td></tr><tr><td>EN, STATE</td><td>Not connected</td><td>Driving needs neither</td></tr></tbody></table>
<div class="note warn"><b>Warning:</b> Do not feed 5 V straight into RXD. The divider looks optional until the module stops answering.</div>
<table><thead><tr><th>At a glance</th><th>Answer</th></tr></thead><tbody><tr><td>Module supply</td><td>3.6 to 6 V on breakout</td></tr><tr><td>Serial pins</td><td>0 receive, 1 transmit</td></tr><tr><td>Data speed</td><td>9600 baud default</td></tr><tr><td>Driver rating</td><td>46 V, 2 A per channel</td></tr><tr><td>Pairing code</td><td>1234 on most boards</td></tr><tr><td>Drive letters</td><td>F B L R plus S stop</td></tr></tbody></table>
[[tool:led-resistor-calculator]]
<h2>Motors need a driver between them and the board</h2>
<figure class="fig"><img src="https://tse1.mm.bing.net/th?q=L298N+driver+board+with+heatsink+close+up&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="L298N driver board with heatsink close up" loading="lazy" decoding="async"></figure>
<p>An Arduino pin cannot run a motor. Motors pull far more current than a logic pin can give. An L298N sits in the middle and handles the heavy switching for you.</p>
<p>Inside sits a dual H-bridge, two switch sets that can reverse current through a motor. STMicroelectronics rates the L298 chip for an operating supply up to <strong>46 V</strong> and <strong>2 A DC per channel</strong>. Your module board sets lower practical limits, so follow the markings printed beside its terminals.</p>
<p>Wire the left motor across <code>OUT1</code> and <code>OUT2</code>. Put the right motor across <code>OUT3</code> and <code>OUT4</code>. Four inputs, <code>IN1</code> to <code>IN4</code>, tell each bridge which way to push. Enable pins ENA and ENB decide whether each side runs at all.</p>
<p>Keep both enable jumpers fitted for full-speed driving. Pull a jumper only for speed control from a PWM pin. PWM means rapid on-off pulses averaging to a lower speed.</p>
<p>Size the motor supply from the motor datasheet, not from hope. Find the stall current, the draw when the shaft cannot turn. Compare that figure with the driver limit and the supply rating, and leave clear headroom above it.</p>
<p>Run the motors from a separate supply matched to their rated voltage. Never run them from the Arduino <code>5V</code> pin. Join the two grounds together so the control signals still make sense.</p>
<ul><li>Sketch pins here: <code>IN1</code> on <code>7</code>, <code>IN2</code> on <code>8</code>, <code>IN3</code> on <code>9</code>, <code>IN4</code> on <code>10</code></li><li>Pack example: four AA cells suit many small motors, after you check the motor rating</li><li>Board power: USB power bank on the bench, barrel jack supply inside Arduino&#x27;s stated range for driving</li><li>Wire order: join grounds first, then motor leads, then logic inputs last</li></ul>
<h2>Letters from your phone become wheel moves</h2>
<p>Sketch code, the program you upload to the board, does one job in a loop. It waits for a character, reads it, and sets the four direction inputs. Simple beats clever here. You can debug a letter. You cannot debug a mystery packet.</p>
<p>Use <strong>9600 baud</strong> in <code>Serial.begin()</code>, matching the module in data mode. Read with <code>Serial.available()</code> first, then take one character with <code>Serial.read()</code>. Echo it back with <code>Serial.println()</code> while you build, so you can see what arrived.</p>
<p>Five letters cover a small car well. Each letter sets the four inputs to one fixed pattern in the sketch below. Treat S as your stop letter from the start, not as an afterthought.</p>
<p>Map phone buttons to those letters instead of typing while you drive. Set the app to send plain ASCII text, one character per tap. Send S when your finger lifts, so the car stops with you.</p>
<p>This example puts the direction inputs on pins <code>7</code>, <code>8</code>, <code>9</code> and <code>10</code>. Change the four numbers at the top if your wiring differs. Keep the letter meanings the same in the app and the sketch, or left becomes right at speed.</p>
<p><b>Bluetooth car sketch for Uno and L298N</b></p><pre><code>const int IN1 = 7;
const int IN2 = 8;
const int IN3 = 9;
const int IN4 = 10;

void setMotors(int a1, int a2, int b1, int b2) {
  digitalWrite(IN1, a1);
  digitalWrite(IN2, a2);
  digitalWrite(IN3, b1);
  digitalWrite(IN4, b2);
}

void setup() {
  Serial.begin(9600);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  setMotors(LOW, LOW, LOW, LOW);
}

void loop() {
  if (!Serial.available()) return;
  char cmd = Serial.read();
  Serial.println(cmd);
  if (cmd == &#x27;F&#x27;) setMotors(HIGH, LOW, HIGH, LOW);
  else if (cmd == &#x27;B&#x27;) setMotors(LOW, HIGH, LOW, HIGH);
  else if (cmd == &#x27;L&#x27;) setMotors(LOW, HIGH, HIGH, LOW);
  else if (cmd == &#x27;R&#x27;) setMotors(HIGH, LOW, LOW, HIGH);
  else setMotors(LOW, LOW, LOW, LOW);
}</code></pre>
[[amazon:Arduino Uno starter kit|Arduino starter kit|First-time builders who need the board, wires and resistors in one box.]]
[[amazon:L298N motor driver module|L298N driver module|Anyone wiring two small DC motors who needs the ready-made driver board.]]
<h2>Prove the link works before the wheels can turn</h2>
<p>Build in stages and faults stay small. Prove the wireless link on its own first, with the driver still off the bench. A letter that arrives cleanly on screen cannot be a motor fault later.</p>
<p>Upload the sketch with the pin <code>0</code> wire lifted, then reconnect the module. Open the Serial Monitor at <strong>9600 baud</strong>. Pair the phone from its Bluetooth settings and pick the device named <code>HC-05</code>.</p>
<p>1234 is the HC-05 default. If it fails, check you paired with HC-05 and not an HC-06, which may use 0000 or 1234. Then open a Bluetooth serial terminal app and connect to the same device.</p>
<p>Send F, B, L, R and S one at a time. Each letter should echo in the monitor exactly as sent. Wrong characters point to a speed mismatch. Silence points to wiring. Fix it now, while the car cannot run away.</p>
<p>Only then power down and add the driver, motors and motor supply. Recheck every terminal against your wiring list before power returns. Raise the wheels on a stand for the first powered run, so a reversed wire spins air, not furniture.</p>
<p>Stay close for that first run. Walls and bodies cut Bluetooth range fast, and a dropout looks exactly like a wiring fault. Lower the car once every letter moves the right wheels the right way.</p>
<ol class="steps"><li>Upload the sketch, then reconnect the module wires.</li><li>Pair the phone with <code>HC-05</code> using code <code>1234</code>.</li><li>Connect from a Bluetooth serial terminal app.</li><li>Send each drive letter and watch the echo.</li><li>Power down, wire the driver, and recheck terminals.</li><li>Run the raised-wheel check before floor driving.</li></ol>
<div class="note"><b>Rule of thumb:</b> Raise the wheels for the first powered run, so a wiring error spins air instead of sending the car across the bench.</div>
<h2>When pairing fails or a wheel goes backwards</h2>
<p>Faults here follow clear patterns once you know them. Work from power to wires to code, in that order. Random part swapping wastes an evening and teaches you nothing.</p>
<p>No pairing at all usually means no power or too much distance. Check VCC and ground with the board powered. Move the phone beside the module and try again. Confirm you picked <code>HC-05</code>, not a neighbour&#x27;s device.</p>
<p>Pairing that succeeds with no data means the serial cross is wrong. Module transmit must land on board receive. Check the divider sits on the board-to-module wire, not the other way round. Confirm <strong>9600 baud</strong> at both ends.</p>
<p>Errors during upload with the module attached are normal on pins <code>0</code> and <code>1</code>. Those pins serve USB and the module at once. Lift the pin <code>0</code> wire, upload, and reconnect. A software serial library on other pins avoids the clash, at some cost in reliability.</p>
<p>One wheel running backwards needs one change, not three. Reverse that motor&#x27;s pair of leads, or invert its two inputs in code. Pick one fix and test again. Changing both puts you back where you started.</p>
<p>Resets, jitter or a hot driver mean the power side is struggling. Confirm the motors run from their own supply with a shared ground. Stop the run and let everything cool before you touch the wiring again.</p>
<h2>Your bluetooth controlled car with Arduino and HC-05, end to end</h2>
<figure class="fig"><img src="https://tse1.mm.bing.net/th?q=Finished+small+robot+car+on+floor&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="Finished small robot car on floor" loading="lazy" decoding="async"></figure>
<p>Lay the parts out before any wire goes in. You need an Uno, an HC-05 breakout and an L298N module. Add two small DC motors, a chassis with wheels, a battery holder, jumper wires and the two divider resistors.</p>
<p>Assembly runs in a fixed order. Motors and driver mount on the chassis first, then the board and module go on top. Grounds join before supplies connect, and the sketch uploads last, with the pin <code>0</code> wire lifted.</p>
<p>A common belief says the phone drives the car. It does not. Your phone only sends letters, and the sketch decides what each letter means. Change the sketch and the same app drives a different car.</p>
<p>Another belief says more supply volts always mean more speed. Past the motor rating, extra volts mostly mean heat and wear. Match the supply to the motor datasheet and let the driver stay inside its own limits.</p>
<p>Speed control comes later, when basic driving feels boring. Move ENA and ENB to PWM pins and add letters for slow and fast. Sensors can follow the same way, without touching the wireless link at all.</p>
<p>Short wires and labelled terminals keep faults easy to trace later. Change one thing at a time, test it, and only then move on to the next improvement.</p>
<ul><li>Before you decide: confirm your board is a breakout, not a bare module</li><li>Before you decide: read the stall current in your motor datasheet</li><li>Before you decide: pick four free digital pins for the driver inputs</li><li>Before you decide: choose a terminal app that sends single ASCII letters</li></ul>
<h2>Quick answers</h2>
<h3>Can I use pins other than 0 and 1 for the HC-05?</h3>
<p>Yes. SoftwareSerial can run the link on other digital pins and leaves USB uploads alone. Hardware serial on pins <code>0</code> and <code>1</code> stays the steadier choice for a first build.</p>
<h3>Why does RXD need a divider if VCC takes 5 V?</h3>
<p>Power and logic levels are separate things. Your breakout regulates its supply down inside, but the receive input still expects <strong>3.3 V</strong> logic. A divider protects that input from the Uno transmit pin.</p>
<h3>What if my motor stall current beats the driver limit?</h3>
<p>Pick a different motor or a stronger driver. Running past the per-channel rating overheats the chip and can damage it. Motor and driver datasheets must agree before you wire anything.</p>
<h3>Do I need AT mode to drive the car?</h3>
<p>No. Data mode at <strong>9600 baud</strong> drives the car with factory settings. AT mode only changes settings such as the device name, pairing code or serial speed.</p>
<h3>How do I add speed control later?</h3>
<p>Remove an enable jumper and wire that enable pin to a PWM output. Then use <code>analogWrite()</code> in the sketch to set the level. Add new letters for slow and fast so the phone controls stay simple.</p>
