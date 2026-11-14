---
title: How to control a servo motor with Arduino: the SG90 way
seo_title: Control a servo motor with Arduino: wiring, code, fixes
description: Wire an SG90 servo to an Arduino Uno, move it with three lines of code, and fix the jitter and resets that come from weak power. Wiring, code and checks.
date: 2026-11-14T16:03:00
updated: 2026-11-14T16:03:00
category: robotics-diy
desk: science
image: https://tse1.mm.bing.net/th?q=how+to+control+a+servo+motor+with+arduino&w=820&h=462&c=7&rs=1&p=0&o=5&pid=1.7
image_alt: Small blue SG90 servo motor wired to an Arduino Uno board
source: contentforge
cf_id: 14387
---
<div class="key"><b>In short:</b> Hook the servo up with its three wires. Then use the Servo library: attach pin 9 and call write with an angle from 0 to 180. Power the servo from its own 5V supply when it stalls, not from the Arduino alone.<ul><li>Three wires: ground, power and signal, in that order.</li><li>The Servo library does the timing. You only call attach and write.</li><li>A stalling servo can pull far more than USB gives, so give it its own power.</li><li>Centre the servo with a write of 90 before you screw the horn on.</li></ul></div>
<h2>How to control a servo motor with Arduino</h2>
<p>A servo motor is a small motor with a brain inside. You tell it an angle, and it turns its shaft to that angle and holds it there. A normal motor just spins. A servo knows where it is pointing, which is why robots use it for arms, grippers, steering and camera mounts.</p>
<p>Most beginners start with the SG90. It weighs 9 grams, costs little, and turns about 180 degrees. Its maker, TowerPro, rates it at 1.8 kg-cm of stall torque at 4.8 V. That means it can hold about 1.8 kg hanging 1 cm from its shaft before it gives up. That is enough for a small gripper or a model steering arm, and not enough for a heavy door.</p>
<p>Control works through pulses. The Arduino sends a short pulse 50 times a second, and the width of the pulse sets the angle. A 1 ms pulse means 0 degrees, a 2 ms pulse means 180 degrees, and 1.5 ms sits in the middle at 90 degrees. You never time these pulses yourself. The Servo library does it for you.</p>
<p>This guide uses an Arduino Uno and one SG90. The same steps work on most Arduino boards, and the same code drives bigger servos too. Bigger servos just need a stronger power supply, which section 4 covers.</p>
<table><thead><tr><th>At a glance</th><th>Answer</th></tr></thead><tbody><tr><td>Servo used</td><td>SG90 micro servo, 9 g</td></tr><tr><td>Signal pin</td><td>Any digital pin, 9 here</td></tr><tr><td>Angle range</td><td>0 to 180 degrees</td></tr><tr><td>Supply</td><td>4.8 to 6 V</td></tr><tr><td>Torque at 4.8 V</td><td>1.8 kg-cm stall</td></tr><tr><td>Library</td><td>Servo, built into the IDE</td></tr></tbody></table>
[[tool:led-resistor-calculator]]
<h2>What you need, and how the wires go</h2>
<figure class="fig"><img src="https://tse1.mm.bing.net/th?q=Three+servo+wires+connected+to+Arduino+pins+close+up&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="Three servo wires connected to Arduino pins close up" loading="lazy" decoding="async"></figure>
<p>You need an Arduino Uno, an SG90 servo, a USB cable and three jumper wires. A breadboard helps but is not required. The SG90 comes with a three-wire lead and a small bag of plastic horns. Pick the single-arm horn for your first test, and screw it on only after the servo is centred.</p>
<p>Three wires leave the servo, and each one has a fixed job. Brown is ground, red is power, and orange is the signal. Some clones use black instead of brown, and yellow instead of orange. The order never changes: ground, power, signal, with the signal wire on the outside edge.</p>
<p>Plug the signal wire into any digital pin. Pin 9 is the usual choice because the Servo library examples use it. The red wire goes to 5V and the brown wire to GND. That is the whole circuit for one small servo, and it is enough to test with. The signal runs at 5 V on an Uno. On a 3.3 V board, check that your servo accepts a 3.3 V signal first.</p>
<p>Double-check the red wire before you plug in the USB cable. A servo wired backwards can burn out in seconds. If your servo has a different colour order, read the label on its case.</p>
<table><caption>SG90 wiring to an Arduino Uno</caption><thead><tr><th>Servo wire</th><th>Colour</th><th>Arduino pin</th></tr></thead><tbody><tr><td>Ground</td><td>Brown (or black)</td><td>GND</td></tr><tr><td>Power</td><td>Red</td><td>5V</td></tr><tr><td>Signal</td><td>Orange (or yellow)</td><td>Digital pin 9</td></tr></tbody></table>
<div class="note warn"><b>Warning:</b> Never power a servo from an Arduino digital pin. A stalled servo can draw far more current than the board or USB can supply. Use a dedicated power source and a shared ground instead.</div>
<h2>The code: three lines that move a servo</h2>
<p>Servo support comes built into the Arduino IDE, so there is nothing to install. Open the IDE, plug in the Uno, and pick the right board and port from the Tools menu. The code below is the classic sweep example, and it is the fastest way to see your servo alive.</p>
<p>Two lines do the real work. The attach line ties the servo to pin 9, and the write line tells it where to point. The loop then steps the angle from 0 to 180 and back, one degree at a time, with a 15 ms pause at each step. That pause is what makes the sweep smooth instead of a single jump.</p>
<p>Upload the sketch and watch the horn. It should swing slowly from one end to the other and back again. If it only twitches, check the wiring first, then the power. A servo that buzzes without moving is usually starved of current, not broken.</p>
<p>Once the sweep works, try fixed angles. A write of 0 points one way, 90 points to the middle, and 180 points the other way. Those three numbers are your test points for every project after this one. Use them for a gripper that opens and closes, a flag that waves, or a lock that turns.</p>
<ol class="steps"><li>Open the Arduino IDE and start a new sketch. Type the code below, or open File, then Examples, then Servo, then Sweep for the same sketch.</li><li>Check the board and port in the Tools menu. Pick Arduino Uno, then the port your board appears on.</li><li>Click Upload. The servo should start sweeping from 0 to 180 degrees and back.</li><li>Change the delay to 50 and upload again. The sweep slows down. Change it to 5 and the sweep speeds up.</li></ol>
<p><b>Sweep the servo from 0 to 180 degrees and back</b></p><pre><code>#include &lt;Servo.h&gt;

Servo myServo;
int angle = 0;

void setup() {
  myServo.attach(9);
}

void loop() {
  for (angle = 0; angle &lt;= 180; angle += 1) {
    myServo.write(angle);
    delay(15);
  }
  for (angle = 180; angle &gt;= 0; angle -= 1) {
    myServo.write(angle);
    delay(15);
  }
}</code></pre>
[[amazon:Arduino Uno starter kit with servo motor|Arduino starter kit|A kit with an Uno, an SG90 servo and jumper wires has everything this guide uses in one box.]]
[[amazon:SG90 micro servo motor pack|SG90 servo pack|A pack of SG90 servos suits readers who want a spare, or one servo per joint on a small robot arm.]]
<h2>Power it right, or it will jitter and reset</h2>
<figure class="fig"><img src="https://tse2.mm.bing.net/th?q=Servo+powered+by+a+separate+battery+pack+beside+Arduino&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="Servo powered by a separate battery pack beside Arduino" loading="lazy" decoding="async"></figure>
<p>Power is where most servo projects fail. A typical PC USB 2.0 port can supply up to 500 mA, and the Arduino shares that current when it runs from USB. A small hobby servo draws tens to a few hundred milliamps while it moves, and several hundred milliamps or more when it stalls. Two servos stalling at once can brown out the board, and the Arduino will reset.</p>
<p>Fix it with a separate supply for the servo. Four AA batteries give about 6 V, which sits inside the SG90 range of 4.8 to 6 V. Connect the battery pack&#x27;s positive wire to the servo&#x27;s red wire, and its negative wire to the servo&#x27;s brown wire and to the Arduino GND. That shared ground is the part people forget, and without it nothing works.</p>
<p>Watch for the warning signs. A servo that jitters, buzzes or twitches at rest is usually underpowered. An Arduino that resets when the servo moves is almost always a power problem. A servo that gets hot is stalled against a load it cannot move, so free the arm or use a stronger servo. Never feed a servo more than 6 V, and never power it from the Arduino Vin pin.</p>
<p>Keep the supply close to the servo. Long thin wires drop voltage. A servo at the end of a metre of jumper wire will behave worse than the same servo on short wires. If the arm must sit far from the board, run the power wires thick and short, and keep only the signal wire long.</p>
<div class="note"><b>Tip:</b> Test with no load first. If the servo sweeps smoothly with nothing attached, then struggles once you fit the arm, the problem is the load, not the code.</div>
<h2>Steer it with a knob: a potentiometer in five minutes</h2>
<p>A potentiometer is a knob that acts as a voltage divider. Turn it one way and the Arduino reads near 0, turn it the other way and it reads near 1023. The map function turns that reading into an angle, so the servo follows the knob. It is the simplest way to drive a servo by hand.</p>
<p>Wire the knob with three wires, like the servo. The two outer pins go to 5V and GND, and the middle pin goes to analog pin A0. It does not matter which outer pin gets 5V and which gets GND, but swapping them flips the direction the knob turns the servo.</p>
<p>In the code, the knob reading is mapped to an angle and written to the servo. The 15 ms delay keeps the loop from hammering the servo with new commands faster than it can move. Turn the knob slowly and the servo follows smoothly. Turn it fast and the servo lags a little, then catches up.</p>
<p>This knob setup is the core of many real projects. A camera pan mount is a knob on a bigger scale. A robot arm is three knobs, one per joint. Once the knob drives one servo, adding a second servo is just a second object, a second pin and a second map line.</p>
<p><b>Drive the servo with a potentiometer on A0</b></p><pre><code>#include &lt;Servo.h&gt;

Servo myServo;
int potPin = A0;
int angle = 0;

void setup() {
  myServo.attach(9);
}

void loop() {
  int reading = analogRead(potPin);
  angle = map(reading, 0, 1023, 0, 180);
  myServo.write(angle);
  delay(15);
}</code></pre>
<h2>Common mistakes that make servos jitter, buzz or stall</h2>
<p>Powering the servo from the Arduino 5V pin while it carries a load is the most common mistake. It works on the bench with no load, then fails the moment the arm meets resistance. The board resets, the servo twitches, and the code gets blamed. When the bench test passes but the loaded arm fails, power is the first suspect.</p>
<p>Fitting the horn before centring the servo is the second mistake. The servo powers up at its last commanded angle, which is rarely the middle. Centre it first with a write of 90, then fit the horn pointing where you want it.</p>
<p>Asking for angles the servo cannot reach is the third mistake. A standard SG90 turns about 180 degrees, and many clones only reach 160 to 170. Code that writes 0 and 180 will push the arm against its end stops, and the servo will buzz and draw stall current. Test your servo&#x27;s real limits and stay a few degrees inside them.</p>
<p>Treating a positional servo like a wheel is the last mistake. A normal servo cannot spin all the way round. If your project needs endless rotation, buy a continuous rotation servo instead. Its write command sets speed, not angle, and it never knows where it is pointing.</p>
<ul><li>Bench test with no load: if it sweeps clean, the code and wiring are fine.</li><li>Jitter at rest: weak power, or a signal wire running beside a motor wire.</li><li>Board resets on movement: the supply cannot keep up with the servo under load.</li><li>Servo buzzes at the ends: the code asks for angles past the real limits.</li></ul>
<h2>Quick answers</h2>
<h3>Can I power an SG90 servo from the Arduino 5V pin?</h3>
<p>For a quick test with no load, yes, and many first sketches do exactly that. For real work, no. A stalling servo can draw several hundred milliamps, more than USB power gives the board. Use a separate 4.8 to 6 V supply for the servo.</p>
<h3>Which pin should the servo signal wire use?</h3>
<p>Any digital pin works, because the Servo library generates the signal in software. Pin 9 is the usual choice, since the library examples use it. On most Arduino boards the library can drive up to 12 servos at once, each on its own pin.</p>
<h3>Why does my servo jitter or twitch?</h3>
<p>Weak power is the usual cause. Check that the servo supply can give the current the servo wants, and that the ground is shared. A loose signal wire also causes twitching, as does a signal wire running beside a motor or power wire. Short, firm connections fix most of it.</p>
<h3>Can a servo spin all the way round?</h3>
<p>A standard positional servo cannot. It turns about 180 degrees and stops. For endless rotation, buy a continuous rotation servo. Its write command sets the speed and direction instead of an angle, and it has no sense of position.</p>
<h3>How do I make the servo move more slowly?</h3>
<p>Move it in small steps with a pause between them. The sweep example steps one degree at a time with a 15 ms delay. Raise the delay to slow the sweep, or lower it to speed it up. A bigger step, such as five degrees, makes the motion visibly jerky.</p>
