# Introduction to Electrical Engineering and Computer Science

## Modularity, Abstraction, and Modeling

How can humans build complex systems that are at a scale of complexity that exceeds their ability to retain the details of the components and details of that system?

Modularity is the idea of building components that can be reused.

Abstraction is the idea that after constructing a module, the details of how the module works can be ignored.

By composing several basic modules into a a new single module, we can create an additional layer of abstraction.

A model is how this abstracted module is described. One could describe a digital watch in terms of clock behavior, or in terms of voltage and currents in the circuit, or in in terms of heat generated at different parts of the circuit. These are all examples of different models. A model is a way of exposing different aspects of the reality of the module.

## An Abstraction Hierarchy of Mechanisms

When dealing with a set of design problems one of the most important things to do is standardize on a _basis set_. Using the same basis set of components as other designers allows to avoid having to reinvent techniques yourself, and allows other people to understand and/or modify your design.

The possible design space can be limited by standardizing on:

- A basis set of _primitive components_
- Combining primitive components to make more complex systems
- "Packaging" or abstracting pieces of design so they can be reused, and essentially creating a new primitive set
- Capturing common patterns of abstractions (e.g. abstracting the abstraction)

Thus complicated design problems can become manageable using a primitive-combination-abstraction-pattern (PCAP) approach.

## Circuits

Electronic circuits are built out of a basis set of primitive components including voltage sources, resistors, capacitors, inductors, and transistors.

The method of combining circuit elements is to connect their terminals with conductors (wires, crimps, solder, etc.). This is called wiring.

Thus, if designing a machine that responds to an external environment you might have a resistance that varies with light level, as well as voltage that does the same.

The formula for this could be expressed as:

`voltage_out = (resistance_b / (resistance_a + resistance_b )) * voltage_input`

Using this abstraction we can understand the problem we are trying to solve better. For example, if `resistance_a` varies based on the amount of light shining in, then we can understand that this will affect `voltage_out`, creating a measurable and defined output.

One complication is if we want this machine to interact with other systems we introduce new inputs and outputs to our formula. So if a motor were connected to this circuit, it would change the voltage, so we would no longer be ablet o maintain the voltage different to drive it. Modularity has not been achieved. This is a problem because now two different people working on the problem cannot design a solution without understanding the entirety of the problem.

One solution would be to augment this circuit with an additoonal component to "buffer" or "isolate" one part of the circuit that allows it to be combined with other parts more easily. A primitive like an op-amp (a DC-coupled electronic voltage amplifier) allows this sort of process.

## Digital Circuits

With analog circuits there is a large amount of freedom to create different types of circuits, but this freedom increases the possibility space which makes design more challenging in terms of the amount of choices to consider. Digital circuits allow a strong abstraction where voltages on the terminal are thought of as either low or high (0 or 1). The basis set of elements are gates (built out of cimpler analog circuit components such as transistors and resistors). This makes digital circuits simple, versatile, inexpensive, and makes combining them easier to reason about. Digital watches, calculators, and computers are all built with these components.
