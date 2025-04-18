# Cooling Fan Controller
Cooling fan controller for Victron Orion DC-DC Charger in Vanessa.

Based on the what I've discovered whilst prototyping:
* [Temperature Sensor Prototype](https://github.com/daipie64/Temperature-Sensor-Prototype)
* [Temperature Controlled Fan Prototype](https://github.com/daipie64/Temperature-Controlled-Fan-Prototype)

## Inspiration
[PWM Fan Project](https://www.youtube.com/watch?v=yqBcSsRsv4I&list=PLWRTMby105bi6HiwsOhd--TAUyEIyOeb6).

## Components
* ESP8266 D1 Mini
  * The MCU to control the fan and read sensors.
* 12V to 5V regulator
  * [Positive Voltage Regulator 5.5-32V to 5V1A Buck Module](https://kunkune.co.uk/shop/dc-to-dc-converters/positive-voltage-regulator-5-5-32v-to-5v1a-buck-module/)
* 12V Fan
  * These are [12V 2-wire 6cm 6015 mini fans from Ali Express](https://www.aliexpress.com/item/33019293160.html?spm=a2g0o.order_list.order_list_main.59.7a5f1802asBSGx).
  * Voltage: 12V
  * Current Rating: 0.18A +/-10%
  * Speed: 4800RPM +/-10%
  * Connector: 2pin (XH2.54)
* 1N4007 Diode
  * Flyback diode to protect against voltage spikes.
  * The fans are inductive loads so the circuit should be protected by a "flyback diode".
  * IN4007
* Dallas DS18B20
  * Measures the Victron Orion's temperature.
  * 10kΩ Resistor – Pull-up resistor for the DS18B20.
* DHT11
  * Measures ambient temperature and humidity.
  * 10kΩ Resistor – Pull-up resistor for the DHT11.
* IRLZ34N MOSFET
  * Logic level mosfet.
  * Switches the fan on and off.
  * Gate Inrush Resistor 470Ω – Limits current into the MOSFET’s gate.
  * [IRLZ34N](https://www.infineon.com/dgdl/Infineon-IRLZ34N-DataSheet-v01_01-EN.pdf?fileId=5546d462533600a40153567206892720).
  * The IRLZ34N MOSFET can be used to switch a 12V, 0.2A fan. The IRLZ34N is a logic-level MOSFET with a low gate threshold voltage, which makes it suitable for switching with a microcontroller or other low-voltage logic. It can handle much higher currents and voltages than your fan requires, so it will work well for your application.
  * Gate Discharge Resistor. A "pull-down resistor" between the gate of the MOSFET and ground is a common practice to ensure that the gate is pulled to ground when there is no active drive signal, preventing the MOSFET from turning on unintentionally due to stray voltages or noise. A typical value for this pull-down resistor is between 10k ohms and 100k ohms. This value is high enough to not interfere with the gate drive but low enough to effectively pull the gate to ground when needed. It ensures the MOSFET remains off when there is no driving signal applied, which is crucial for reliable operation.
 
## Generating the code.
I used ChatGPT to generate the code. The prompt & subsequent chat dialog can be seen here. [ChatGPT: Temp controlled fan](https://chatgpt.com/share/67d2d8a2-fc4c-8001-b3f6-e2f3b498e3e5)

After ChatGPT had created the initial code I went back to the char and asked it to generate algorithms that set fan speed update interval based on the temperaure. The idea being that when the temerature is close to ambient then the update interval does not need to be that often .... but does increase when the temperature is perhaps above optimal.

ChatGPT went on to suggest some other cool things:
* Lovelace dashboard cards to control everything visually
* Then showed me how to make these cards "conditional" - for example, hide unused sliders when a different model is selected.
* A toggle to enable/disable adaptive control
* Logging suggestions to track which model is active and how it performs
* A graph card to visualize interval & fan speed trends over time - with color-coding & advanced graph customization
* Then showed me how to install mini-graph-card and create sleek sparkline graphs with shaded thresholds
* Then a "model performance comparison" card that logs/visualizes which model was active over time

Here's the chat history for that [ChatGPT: add adaptive update algorithms plus some other cool things](https://chatgpt.com/g/g-HaRByhtTl-home-assistant-assistant/c/67d7064d-9ef4-8001-b204-753f9bf6a477)

Quite a few things didn't work with the version of ESPHome that I was running - I guess the model was for an older version. Anyway, it gave ne some great ideas and the first version adapted the speed of the fans linearly ... I may go on to look at the other algorithms - we'll see :-)

## Runtime Current Draw
I measured the following current draw.

With fans at idle the current draw was 0.03A, 0.36W.

With fans at full spped the current draw was 0.35A, 4.2W.



