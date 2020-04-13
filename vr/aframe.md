# A-Frame

A-Frame is a web framework that is used to develop web VR applications. It uses a declarative HTML structure, an entity-component architecture, and is cross-compatible with various VR devices.

## The Entity Component System Design Pattern

The Entity Component System Design Pattern (ECS) is used in game development, and is similarly useful for developing VR applications. It defines entities as container objects that components become attached to. Components are modular pieces of data that are attached to components. Systems are the global scope and management of components.

## Using A-Frame

To create a VR application using A-Frame, you will want to include the following script tag in the `head` element of your webpage:

`<script src="https://aframe.io/releases/0.9.0/aframe.min.js"></script>`

It can also be helpful to include resources like the A-Frame environment component as well.

`<script src="https://unpkg.com/aframe-environment-component@1.1.0/dist/aframe-environment-component.min.js"></script>`

## Scene

In your `body` element you include an `<a-scene>` tag. The scene is the global root object that contains all the entities. Among other things, the `<a-scene>` element creates a canvas and default camera and lights.

### Scene Components

Components can be attached to scene elements. Perhaps one of the most useful components to attach to a scene for developers is that `stats` component which allows you to view performance stats in your scene.

`<a-scene stats>`

## Entities

In A-Frame entities are attached with `position`, `rotation`, and `scale` components.

## Components

Components are either *single-property* or *multi-property*. A single-property component has data that consists of a single value, or set of values. A multi-property component consists of multiple properties, and this resembles a JavaScript object. For example, when assigning a `light` component to an entity, it could have the following value:

`<a-entity light="type: point; color: crimson"></a-entity>`

### Position

The position component takes 3 signed integers as its arguments for the position of the entity it is attached to. The three values correspond to the x, y, and z axis respectively. Note that positioning is always relative to the parent entity.

### Rotation

The rotation component defines the orientation of an entity in degrees along the x, y, and z axis. Rotation values of an entity are always relative to the parent entity.

### Scale

The scale component defines a shrinking, stetching, or skewing transformation of an entity along the x, y, and z axis. When these values are negative it results in a reflection. Scale is relative to the parent entity.

## Resources

- [Supercraft](https://supermedium.com/supercraft) is a tool for creating VR objects in VR that can be loaded into an A-Frame environment.

- [A-Painter](https://aframe.io/a-painter) a painting tool created by Mozilla similar to Google's Tilt Brush with an API for creating your own brushes.

- [Experimental Physics Component](https://github.com/ngokevin/aframe-physics-components)
