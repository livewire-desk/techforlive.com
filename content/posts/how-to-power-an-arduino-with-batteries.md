---
title: How to power an Arduino with batteries: the safe way
seo_title: Power an Arduino with batteries: safe wiring guide
description: Run an Arduino Uno or Nano from batteries: the three power inputs, which battery fits your project, wiring steps and a simple runtime estimate.
date: 2026-11-15T15:00:00
updated: 2026-11-15T15:00:00
category: robotics-diy
desk: science
image: https://tse1.mm.bing.net/th?q=how+to+power+an+arduino+with+batteries&w=820&h=462&c=7&rs=1&p=0&o=5&pid=1.7
image_alt: Arduino Uno board powered by a battery pack on a desk
source: contentforge
cf_id: 14712
---
<div class="key"><b>In short:</b> To power an Arduino with batteries, feed 7 to 12 volts into the barrel jack or the Vin pin. The board&#x27;s regulator trims it to 5 volts. Or plug a USB power bank into the USB port for a steady 5 volts. Match the battery to the project. For a blinking LED, a 9 volt battery is enough. Motors and radios need AA cells or a lithium pack.<ul><li>7 to 12 volts into Vin or the barrel jack; 5 volts only into USB or a regulated 5V pin.</li><li>On a 9 volt battery, a bare board runs for hours; a motor needs more.</li><li>Work out runtime as battery mAh divided by draw in mA.</li><li>Never feed more than 12 volts into Vin, and never feed the 5V pin while USB is plugged in.</li></ul></div>
<h2>How to power an Arduino with batteries</h2>
<p>Before it can do anything, an Arduino needs power. On your desk, that power comes from the USB cable. Out in the world, it has to come from a battery. This guide shows you how to power an Arduino with batteries, which battery to pick, and how to wire it without damaging the board.</p>
<p>Here is the short version. Battery packs go into the barrel jack or the Vin pin. For a clean, steady 5 volts, use the USB port instead.</p>
<p>Voltage is the push behind the electricity. Think of it like water pressure in a hose: too little and nothing flows, too much and the hose bursts. Your Arduino wants 5 volts inside. Inside, a part called a voltage regulator trims a higher voltage down to 5. Every regulator needs a little headroom, called dropout, so the input must sit above 5 volts. Feed it 7 to 12 volts and the regulator does its job. Less makes the board act strangely. More makes the regulator hot.</p>
<p>Current is the other half of the story. It is the amount of electricity flowing, measured in milliamps, shortened to mA. One small LED needs about 20 mA. The Uno board on its own takes about 50 mA. When it pushes hard, a servo motor can want several hundred mA, so your battery must cover everything at once.</p>
<table><thead><tr><th>At a glance</th><th>Answer</th></tr></thead><tbody><tr><td>Board used</td><td>Arduino Uno; Nano follows the same rules</td></tr><tr><td>Vin range</td><td>7 to 12 volts recommended</td></tr><tr><td>USB input</td><td>5 volts</td></tr><tr><td>3.3V pin limit</td><td>50 mA maximum</td></tr><tr><td>Board draw</td><td>About 50 mA on its own</td></tr><tr><td>9V battery capacity</td><td>A few hundred mAh, varies by brand</td></tr><tr><td>Runtime rule</td><td>mAh divided by mA gives rough hours</td></tr></tbody></table>
[[tool:battery-life-calculator]]
<h2>The three ways power gets into your board</h2>
<figure class="fig"><img src="https://tse3.mm.bing.net/th?q=Close+up+of+Arduino+Uno+power+jack%2C+Vin+pin+and+USB+port&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="Close up of Arduino Uno power jack, Vin pin and USB port" loading="lazy" decoding="async"></figure>
<p>Four power entrances sit on an Uno, but only three are meant for batteries. The USB port takes 5 volts. The barrel jack and the Vin pin take 7 to 12 volts. The 5V pin is an output, not an input. The official Arduino documentation says feeding power through it bypasses the regulator, and it is not advised.</p>
<p>Barrel jack and Vin feed the same regulator. The jack is the round socket on the edge of the Uno, made for a 2.1 mm centre-positive plug. Vin is the pin next to the 5V pin, and it takes bare wires from a battery holder. The Nano has no barrel jack, so Vin is its only wired input.</p>
<p>USB is the simplest route for a 5 volt supply: a power bank, a phone charger or a computer port all give 5 volts. Most Unos have a resettable fuse on the USB line to guard your computer if something shorts. Its trip rating varies by board revision and clone. If the board keeps resetting on USB power, try a different cable or port before you blame the code.</p>
<table><caption>Arduino power inputs at a glance</caption><thead><tr><th>Input</th><th>Voltage</th><th>Best for</th></tr></thead><tbody><tr><td>USB port</td><td>5 V</td><td>Power bank or phone charger</td></tr><tr><td>Barrel jack</td><td>7 to 12 V</td><td>Battery pack with a plug</td></tr><tr><td>Vin pin</td><td>7 to 12 V</td><td>Bare wires from a battery holder</td></tr><tr><td>5V pin</td><td>5 V regulated only</td><td>Expert use, never raw batteries</td></tr></tbody></table>
<div class="note warn"><b>Warning:</b> Never feed more than 12 volts into Vin or the barrel jack. The regulator turns the extra voltage into heat, and it can overheat and damage the board.</div>
<h2>Pick the battery that fits your project</h2>
<p>Start with what the project draws, not with the battery you happen to own. With one LED blinking, a bare Uno draws about 50 to 70 mA. Add a servo and the total can jump past 500 mA when the servo pushes. The battery must cover the peaks, not just the average.</p>
<p>Most beginners reach for a 9 volt battery first, yet it is also the weakest. Typical alkaline 9 volt cells hold a few hundred mAh, and the exact figure varies by brand and load. It cannot give much current at once. It runs a bare board with a blinking LED for roughly half a day. Keep it for light jobs and short tests.</p>
<p>AA cells are the workhorse for most battery projects. One alkaline AA holds about 2,000 to 3,000 mAh. Four AAs give about 6 volts. Since the onboard regulator needs headroom, Vin is commonly supplied with about 7 to 12 volts, so four cells can be marginal. Six AAs give 9 volts, which sits neatly inside the range. Rechargeable NiMH AAs give 1.2 volts each, so eight of them make 9.6 volts for Vin.</p>
<p>Lithium cells hold the most energy for their size, and they need the most care. One 18650 cell gives 3.7 volts and about 2,500 to 3,500 mAh, but that voltage fits neither Vin nor the 5V pin. You need a small converter board between the cell and the Arduino. Only use cells with a built-in protection circuit.</p>
<ul><li>Blinking LED or simple sensor: a 9 volt battery, or a USB power bank.</li><li>Servo, motor or radio: six AA cells, or a lithium pack with a converter.</li><li>Sensor left outside for weeks: lithium cells plus sleep mode, covered in section 5.</li></ul>
[[amazon:6 AA battery holder with barrel jack plug|6xAA battery holder|A six-cell holder gives 9 volts into Vin, which suits most first battery projects.]]
[[amazon:10000mAh USB power bank|USB power bank|A power bank gives a steady 5 volts through USB, which is the simplest battery route.]]
<h2>Wire a battery pack to your Arduino</h2>
<figure class="fig"><img src="https://tse1.mm.bing.net/th?q=Hands+connecting+a+battery+holder+to+Arduino+Vin+and+ground+pins&amp;w=560&amp;h=380&amp;c=7&amp;rs=1&amp;p=0&amp;o=5&amp;pid=1.7" alt="Hands connecting a battery holder to Arduino Vin and ground pins" loading="lazy" decoding="async"></figure>
<p>Wiring a battery to an Arduino is a five-minute job, but the order matters. Do the wiring with the battery unplugged, check it twice, then connect the power last. With the battery already in, a wrong wire can damage the board before you notice.</p>
<p>Shared ground is the rule people forget. Every part of a circuit needs the same zero point, called ground or GND. The battery&#x27;s black wire, the Arduino GND pin and the ground of any sensor or motor must all join together. Without that shared ground, signals have no reference and nothing works.</p>
<p>Motors and servos get their own supply, never the Arduino&#x27;s regulator. When it stalls, a servo can pull several hundred milliamps, which is more than the small regulator on the Uno can pass without overheating. Give the servo its own 4.8 to 6 volt pack, join the grounds, and send only the signal wire to a digital pin.</p>
<p>Upload sketches over USB as normal. The board picks its power source on its own, so you can leave the battery connected while you upload. One limit: never feed the 5V pin and USB at the same time, as power can flow back into your computer.</p>
<ol class="steps"><li>Pick the input: the barrel jack for a plug, Vin for bare wires, USB for a 5 volt bank.</li><li>Check the voltage with a meter if you have one. It must read between 7 and 12 volts for Vin, or 5 volts for USB.</li><li>Connect the battery holder&#x27;s red wire to Vin, or plug the jack in. Connect the black wire to any GND pin.</li><li>Plug the battery in last, after all the other wires are set. Watch the green power LED come on.</li><li>Upload your sketch over USB as normal, with the battery still connected.</li></ol>
<div class="note"><b>Tip:</b> Put a switch between the battery and the board. It stops the battery draining while the project sits on the bench, and it gives you a quick way to cut power.</div>
<h2>Make one set of batteries last longer</h2>
<p>Doing less is the cheapest way to stretch a battery. Left blinking an LED all day, a board draws power all day. Waking to read a sensor, send the result and sleep again, a board can run for weeks on the same cells. Sleep is the trick, and the Arduino chip has it built in.</p>
<p>Sleep is built into the ATmega328P chip on the Uno and Nano. In power-down mode it stops almost everything and wakes only on an interrupt or a timer. The LowPower library by RocketScream wraps it in one line of code. Install it from the Library Manager, then call powerDown with a sleep time of up to 8 seconds.</p>
<p>One honest warning: sleep saves the chip, not the whole board. The Uno&#x27;s regulator, its USB chip and its power LED keep drawing current even while the chip sleeps. Even in sleep, a full Uno still draws tens of milliamps. For a sensor that must run for months, use a bare chip or a low-power board instead.</p>
<p>Before you buy a bigger battery, work out the runtime on paper. Divide the battery capacity in mAh by the total draw in mA for the rough number of hours. Feeding a 50 mA board, a 600 mAh battery gives about 12 hours.</p>
<p><b>Blink once, then sleep for 8 seconds</b></p><pre><code>#include &lt;LowPower.h&gt;

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH);
  delay(100);
  digitalWrite(LED_BUILTIN, LOW);
  LowPower.powerDown(SLEEP_8S, ADC_OFF, BOD_OFF);
}</code></pre>
<h2>Mistakes that drain batteries or kill boards</h2>
<p>Most battery projects fail on the same few mistakes. The first is feeding the wrong voltage to the wrong pin. Nine volts into the 5V pin will damage the chip, because that pin has no regulator in front of it. Six volts from four fresh AAs is too much for the same pin, and too little for Vin. The board acts strangely as the cells age.</p>
<p>The second mistake is trusting a 9 volt battery with a motor. It looks like plenty of power in a tidy package. Its small capacity and low current make it a poor match for servos, motors and radios. The board resets, the motor stutters, and the builder blames the code.</p>
<p>The last mistake is treating a lithium cell like an AA cell. Without a protection circuit, a lithium cell can get very hot if it is shorted or charged the wrong way. Keep cells in a holder, never loose in a pocket with keys, and use only a charger made for that cell type. If a cell swells, hisses or smells odd, move it outside on a non-flammable surface and leave it alone.</p>
<ul><li>Four AAs into Vin: about 6 volts can be marginal, and the board may act unreliably as the cells droop under load.</li><li>Two power sources into the 5V pin and USB at once: the board can feed power back into your computer.</li><li>Lithium cell with no protection circuit: a short can make it very hot, very fast.</li></ul>
<h2>Quick answers</h2>
<h3>Can I power an Arduino Uno with a 9V battery?</h3>
<p>Yes, for light work. Connect it to Vin or the barrel jack, and the regulator trims 9 volts to 5. One standard 9 volt cell holds a few hundred mAh, so a bare board runs for a few hours.</p>
<h3>Can I plug a battery straight into the 5V pin?</h3>
<p>Only if the supply is a steady, regulated 5 volts. Raw battery packs are not regulated, and 6 volts from four AA cells is too much for the chip.</p>
<h3>How long will my Arduino run on batteries?</h3>
<p>Divide the battery capacity in mAh by the total draw in mA. Feeding a 50 mA board, a 600 mAh cell lasts about 12 hours, in rough terms. Motors, radios and bright LEDs raise the draw and cut that time fast.</p>
<h3>Can I leave the battery connected while I upload code?</h3>
<p>Yes. Uploading works the same with the battery connected, since USB still carries the data. Do not feed the 5V pin and USB at the same time, though, as power can flow back into the computer.</p>
<h3>Will a USB power bank run an Arduino?</h3>
<p>Yes, through the USB port, since a bank gives a steady 5 volts. Some banks switch off when the draw is tiny, so a sleeping board can lose power. Pick a bank that stays on under a light load.</p>
