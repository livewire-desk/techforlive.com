---
title: L298N motor driver explained: wiring, code and fixes
seo_title: L298N motor driver with Arduino: wiring and code
description: Wire an L298N motor driver to an Arduino Uno, drive two DC motors forwards and backwards, and fix the power mistakes that stall most first robots.
date: 2026-11-15T14:08:00
updated: 2026-11-15T14:08:00
category: robotics-diy
desk: science
image: https://tse4.mm.bing.net/th?q=l298n+motor+driver+explained&w=820&h=462&c=7&rs=1&p=0&o=5&pid=1.7
image_alt: Red L298N motor driver module wired to an Arduino Uno
source: contentforge
cf_id: 14710
---
<div class="key"><b>In short:</b> An L298N board sits between an Arduino Uno and two DC motors. The Arduino sends small signals, and the board switches the motor power to turn each motor forwards or backwards, faster or slower. The motors take power from their own battery, not from the Arduino. The chip loses voltage inside itself, so under load the motor sees less than the supply gives.<ul><li>The L298N lets an Arduino drive two DC motors, both ways, from a separate battery.</li><li>IN1 and IN2 set direction. ENA takes PWM for speed.</li><li>The chip keeps some voltage for itself, so feed the motor a couple of volts extra.</li><li>Pull the ENA caps off for speed control, and keep the 5V jumper on only up to 12 volts.</li></ul></div>
<h2>The L298N motor driver, explained simply</h2>
<p>An L298N motor driver is a small board that lets a tiny computer run a real motor. An Arduino Uno can only give a few milliamps from a pin. A DC motor wants hundreds of milliamps, sometimes more. The L298N sits between them and does the heavy lifting.</p>
<p>Inside the board is a chip called the L298, made by STMicroelectronics. It holds two H-bridges, switch layouts that can push current through a motor in either direction. One bridge drives motor A, the other drives motor B. That is why one board runs both wheels of a robot car.</p>
<p>According to the ST L298 datasheet, the chip takes a motor supply of up to 46 volts. Each channel can carry up to 2 amps of steady current. In practice the current is lower, because heat, the module parts and the heatsink all limit it. Module input ratings vary by maker. Some common hobby modules accept about 5 to 35 volts, but check the print on the board you bought.</p>
<p>Think of the L298N as a remote control for power. The Arduino sends small signals that say which way each motor should turn and how fast. The board then connects the motor to the battery in the right direction, at the right speed. The Arduino never touches the motor current.</p>
<table><thead><tr><th>At a glance</th><th>Answer</th></tr></thead><tbody><tr><td>Chip</td><td>L298 dual full-bridge driver, STMicroelectronics</td></tr><tr><td>Motor supply</td><td>5 to 35 V on the module, up to 46 V on the chip</td></tr><tr><td>Current</td><td>2 A steady per channel, 4 A total</td></tr><tr><td>Logic supply</td><td>4.5 to 7 V, 5 V typical</td></tr><tr><td>Voltage drop</td><td>About 1.8 V typical at 1 A</td></tr><tr><td>Enable pins</td><td>ENA and ENB, PWM for speed</td></tr></tbody></table>
[[tool:led-resistor-calculator]]
<h2>How an H-bridge flips a motor around</h2>
<p>A DC motor turns one way when current flows through it in one direction. Swap the two wires round and the same motor turns the other way. An H-bridge is a set of four switches wired in the shape of the letter H. It swaps the wires for you, in software.</p>
<p>Each bridge has two control inputs, called IN1 and IN2 for motor A. Set IN1 high and IN2 low, and the motor turns forward. Flip them and the motor turns backward. Set both the same and the motor stops. The enable pin, ENA, acts as the main switch for that channel.</p>
<p>Speed comes from a trick called PWM, short for pulse width modulation. The enable pin is switched on and off hundreds of times a second. On most of the time, the motor runs near full speed. On only a little of the time, the motor crawls. The Arduino function analogWrite does this, with a number from 0 to 255.</p>
<p>Motor B works the same way, with IN3, IN4 and ENB for its channel. Two bridges on one chip is what makes the L298N the standard choice for a two-wheeled robot. Each wheel gets its own speed and direction, so the robot can turn by running one wheel faster than the other. The chip has no diodes to catch the spike a stopping motor makes, so the module fits them on the board.</p>
<h2>Wiring the L298N to an Arduino Uno</h2>
<figure class="fig"><img src="https://tse1.mm.bing.net/th?q=L298N+module+wired+to+an+Arduino+Uno+with+jumper+wires&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="L298N module wired to an Arduino Uno with jumper wires" loading="lazy" decoding="async"></figure>
<p>You need an Arduino Uno, an L298N module, one or two small DC motors, a battery pack, and six jumper wires. The battery powers the motors. The Arduino powers only its own logic. Keep those two jobs apart from the start and most wiring trouble never happens.</p>
<p>Start with the motor supply. Connect the battery positive to the +12V screw terminal and the battery negative to the GND terminal next to it. The label says 12V, but common modules take about 5 to 35 volts. A 9V battery or six AA cells works for a first test. Fit the motor to the OUT1 and OUT2 terminals.</p>
<p>Now the control wires. Pull the small jumper caps off ENA and ENB, or speed control will not work. Wire ENA to Arduino pin 9, IN1 to pin 8, and IN2 to pin 7. For a second motor, wire ENB to pin 3, IN3 to pin 5, and IN4 to pin 4. Pins 9 and 3 can do PWM, which is why they carry the enable signals. On the Uno, the PWM pins are 3, 5, 6, 9, 10 and 11.</p>
<p>Last, join the grounds. Run a wire from a GND pin on the module to a GND pin on the Arduino. This shared ground is the wire people forget, and without it the signals mean nothing. Leave the 5V-enable jumper in place when the battery is 12 volts or less. The onboard regulator then makes 5 volts for the chip.</p>
<ol class="steps"><li>Connect the battery to the +12V and GND screw terminals.</li><li>Fit motor A to the OUT1 and OUT2 terminals.</li><li>Pull the ENA and ENB jumper caps off the board.</li><li>Wire ENA to pin 9, IN1 to pin 8 and IN2 to pin 7.</li><li>Join the module GND to the Arduino GND.</li></ol>
<table><caption>L298N wiring to an Arduino Uno</caption><thead><tr><th>L298N pin</th><th>Arduino pin</th><th>What it does</th></tr></thead><tbody><tr><td>ENA</td><td>Pin 9 (PWM)</td><td>Speed of motor A</td></tr><tr><td>IN1</td><td>Pin 8</td><td>Direction of motor A</td></tr><tr><td>IN2</td><td>Pin 7</td><td>Direction of motor A</td></tr><tr><td>ENB</td><td>Pin 3 (PWM)</td><td>Speed of motor B</td></tr><tr><td>IN3</td><td>Pin 5</td><td>Direction of motor B</td></tr><tr><td>IN4</td><td>Pin 4</td><td>Direction of motor B</td></tr><tr><td>GND</td><td>GND</td><td>Shared ground</td></tr><tr><td>+12V</td><td>Battery positive</td><td>Motor power in</td></tr><tr><td>OUT1 / OUT2</td><td>Motor A wires</td><td>Motor A output</td></tr></tbody></table>
<div class="note warn"><b>Warning:</b> Never feed 5 volts into the 5V pin while the 5V-enable jumper is still in place. Two supplies fighting can damage the regulator and the Arduino.</div>
[[amazon:L298N motor driver module|L298N driver module|The red module this guide uses, with the heatsink and screw terminals already fitted.]]
[[amazon:Arduino Uno starter kit with DC motors|Arduino Uno starter kit|A kit with an Uno, jumper wires and small DC motors covers the whole build in one box.]]
<h2>The code: speed and direction with PWM</h2>
<p>This sketch drives one motor. It sets the pins up, then runs the motor forward at a medium speed for two seconds, stops it, and runs it backward. The speed number runs from 0, stopped, to 255, full speed. Change the number to change the speed.</p>
<p>Three functions do the work. The pinMode lines in setup mark each pin as an output. The digitalWrite lines set IN1 and IN2, which pick the direction. The analogWrite line sends PWM to ENA, which sets the speed. Upload the sketch and the motor should turn.</p>
<p>If the motor turns the wrong way, swap the two values in the code, or swap the motor wires at the OUT terminals. Both fixes do the same job. If the motor only buzzes, the speed number may be too low to break the motor free. Try 200 first, then lower it once the motor turns.</p>
<p>A second motor needs the same pattern with its own pins. Copy the three lines for ENB, IN3 and IN4, and give them their own speed number. A robot that drives straight needs both motors at the same speed, but the two motors are rarely matched. Nudge one speed by a few points until the robot runs straight.</p>
<p><b>Drive one DC motor forwards, stop, then backwards</b></p><pre><code>const int ENA = 9;
const int IN1 = 8;
const int IN2 = 7;

void setup() {
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
}

void loop() {
  // Forward at a medium speed for two seconds
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  analogWrite(ENA, 200);
  delay(2000);

  // Stop for one second
  analogWrite(ENA, 0);
  delay(1000);

  // Backward at the same speed for two seconds
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  analogWrite(ENA, 200);
  delay(2000);

  analogWrite(ENA, 0);
  delay(1000);
}</code></pre>
<h2>Power rules: the 5V jumper and the 2V drop</h2>
<figure class="fig"><img src="https://tse2.mm.bing.net/th?q=L298N+module+heatsink+close+up+on+a+robot+chassis&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="L298N module heatsink close up on a robot chassis" loading="lazy" decoding="async"></figure>
<p>Voltage is lost inside the L298 chip. The ST L298 datasheet lists a typical total drop of about 1.8 volts at a load of 1 amp. The drop grows as the current rises. Under load, a 12 volt supply may deliver noticeably less to the motor. A 6 volt supply leaves even less headroom, and a small motor may not turn at all.</p>
<p>Lost voltage turns into heat. The drop times the current is power wasted in the chip, and that is why the module ships with a big metal heatsink. The 2 amp figure is a chip limit, not a module promise. If the heatsink is too hot to touch, stop and use a lighter load.</p>
<p>The 5V-enable jumper is the other power rule. With the jumper in place, the onboard regulator turns the motor supply into 5 volts for the chip logic. That works only up to a 12 volt supply. Feed the board more than 12 volts with the jumper still in place and the small regulator cooks. Take the jumper off first, then feed 5 volts into the 5V pin.</p>
<p>Size the battery for the motors, not the Arduino. A 9 volt battery can run a tiny motor for a short test, but it fades fast under load. Six AA cells or a 7.4 volt lithium pack lasts far longer. Pick a supply a couple of volts above the motor rating.</p>
<div class="note"><b>Tip:</b> Test with the wheels off the ground first, on a current-limited supply if you have one. A wiring fault then spins a wheel in the air, not a robot across the room.</div>
<h2>Common mistakes that stop an L298N robot</h2>
<p>No shared ground is the most common fault. The battery, the L298N and the Arduino must all agree on what zero volts is. A motor that sits dead while the code looks right usually has a missing ground wire. The fix is one wire, and section 3 shows where it goes.</p>
<p>Powering the motors from the Arduino 5V pin is the second fault. The Uno 5V pin can give only a small current, and a DC motor wants far more when it starts. The board browns out and resets, or the Uno regulator overheats. Motors always get their own supply.</p>
<p>Leaving the ENA jumper caps on is the third fault. The caps tie ENA and ENB to 5 volts, so both motors run at full speed whatever the code says. Speed control seems dead, and no change to the code helps. Pull the caps off and wire ENA and ENB to PWM pins.</p>
<p>Forgetting the voltage drop is the last fault. A robot that crawls on a 6 volt supply is not broken. The chip takes voltage for itself, so the motor sees less than the supply. Raise the supply a little, within the motor rating, and the same robot runs as expected. Always test with the wheels off the ground first.</p>
<ul><li>Motor dead on arrival: check the battery leads and the +12V terminal first.</li><li>Board resets when the motor starts: the motors are fed from the Arduino supply.</li><li>Speed control does nothing: the ENA and ENB jumper caps are still on.</li><li>Regulator runs hot: the 5V jumper is still on above a 12 volt supply.</li></ul>
<h2>Quick answers</h2>
<h3>Can the L298N run two motors at once?</h3>
<p>Yes, it can. It holds two H-bridges, one per motor. Motor A uses IN1, IN2 and ENA, and motor B uses IN3, IN4 and ENB. Each motor gets its own speed and direction.</p>
<h3>What voltage should I feed an L298N module?</h3>
<p>Common hobby modules take about 5 to 35 volts on the motor input, but ratings vary by maker. A 9 volt battery works for a small test, and six AA cells or a 7.4 volt pack suits a robot car. Keep the 5V jumper on only up to 12 volts.</p>
<h3>Why does my motor run slower than the battery voltage suggests?</h3>
<p>The L298 chip drops voltage inside itself, so the motor sees less than the supply. The drop grows with current, so a hard-working motor sees even less. Feed it a little more than the motor rating, and check the datasheet drop figures against your load.</p>
<h3>Can I use the L298N with a 3.3 volt board?</h3>
<p>It can, with a check first. Level compatibility depends on the chip input thresholds. Many TTL-compatible inputs will register 3.3 volts as high. Check the L298 input threshold in the datasheet and verify on your module before you rely on 3.3 volt logic. The motor supply still needs its own battery. Wire the grounds together as section 3 shows.</p>
<h3>Do I need the heatsink?</h3>
<p>Yes, keep it fitted. The chip turns the lost voltage into heat, and the heatsink sheds it. A stalled motor can heat the chip fast. If the heatsink is too hot to touch, stop and lighten the load.</p>
