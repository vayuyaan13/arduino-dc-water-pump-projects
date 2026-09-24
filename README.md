# Arduino DC Water Pump Projects

A practical guide to controlling a small DC water pump with Arduino for plant watering, water circulation, and simple automation.

## What is a DC water pump?

A DC water pump uses direct-current power to drive a motor that moves water. Hobby pumps are commonly available in low-voltage ratings such as 5V, 6V, 9V, or 12V.

Always check the pump's rated voltage and current before choosing a power supply.

For a broader beginner explanation of pump selection, control, wiring, and applications, see this [DC water pump](https://vayuyaan.com/blog/dc-water-pump-ultimate-guide-for-beginners/) guide.

## Why Arduino should not power the pump directly

An Arduino I/O pin is intended for logic signals, not motor loads. A pump can draw far more current than an I/O pin can safely provide.

Use Arduino as the controller and a suitable external supply for the pump.

## Components

- Arduino Uno
- DC water pump
- Pump-rated external DC supply
- Relay module or suitable logic-level MOSFET
- Tubing and water container
- Jumper wires
- Flyback diode for a bare transistor/MOSFET circuit

## Relay control

A relay module can switch a separate pump supply.

Typical control wiring:

- Arduino digital pin 7 → relay input
- Relay module power → according to its specifications
- External pump supply → relay contacts
- Pump → external supply through the switched contacts

Check the relay module's terminal labels and whether its input is active HIGH or active LOW.

## Basic Arduino code

~~~cpp
const int PUMP_PIN = 7;

void setup() {
  pinMode(PUMP_PIN, OUTPUT);
  digitalWrite(PUMP_PIN, LOW);
}

void loop() {
  digitalWrite(PUMP_PIN, HIGH);
  delay(5000);

  digitalWrite(PUMP_PIN, LOW);
  delay(10000);
}
~~~

Some relay modules are active-low. Adjust the logic if necessary.

## MOSFET control

For frequent switching, a suitable logic-level N-channel MOSFET can be more efficient than a mechanical relay.

A typical low-side circuit uses:

- Pump positive → external supply positive
- Pump negative → MOSFET drain
- MOSFET source → supply ground
- Arduino output → MOSFET gate
- Gate pull-down resistor
- Flyback diode across the pump
- Common ground between Arduino and the external supply

Choose the MOSFET and diode for the pump's actual voltage and current.

## Automatic plant watering

Combine the pump with a soil-moisture sensor:

1. Read soil moisture.
2. Compare it with a calibrated dry-soil threshold.
3. Start the pump when soil is too dry.
4. Run it for a short interval.
5. Wait for water to spread.
6. Measure again.

This is safer than running the pump continuously.

## Water-level protection

Add a float switch or water-level sensor to the source tank. Stop the pump when the tank is empty to prevent dry running.

## Troubleshooting

**Pump does not start:** verify voltage, current capacity, polarity, control wiring, and relay/MOSFET connections.

**Arduino resets when pump starts:** use a suitable separate pump supply, improve wiring, and suppress motor noise.

**Relay clicks but pump stays off:** check the relay contact path and pump supply.

**Weak water flow:** inspect tubing, inlet blockage, water level, pump orientation, and supply voltage.

**MOSFET gets hot:** verify the MOSFET is logic-level at the Arduino drive voltage and adequately rated.

## Safety

Keep water away from exposed electronics. Use appropriately rated low-voltage supplies for hobby projects. Do not use this circuit as a direct method for switching mains-powered pumps.

## Practical projects

- Automatic plant watering
- Mini fountain
- Aquarium circulation
- Water-level automation
- Cooling-water circulation
- Timer-based irrigation

## Testing checklist

- Confirm pump voltage.
- Confirm supply polarity.
- Check current requirement.
- Test the controller before connecting the pump.
- Check for leaks.
- Keep electronics dry.
- Add a software timeout.

## License

This project is provided under the repository license.
