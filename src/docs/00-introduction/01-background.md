---
name: Background
order: 1
---

# Background
Atlas is a modular image viewer, based on the conceptual model set by the [IIIF Presentation Framework](https://iiif.io/api/presentation/3.0/). It can render deep zoom images on an HTML Canvas, or static thumbnail images in standard Image tags.

## Philosophy
For something to be truly useful, it has to be **very easy to use** but as **powerful as you need** it to be.
Throughout this documentation you will see very high-level APIs to work quickly with the viewer to whip
something together, but also a deeper dive, peeling back the abstractions. In each of the libraries
all of the internals are exposed. From the primitive building blocks, you could build a different
viewer entirely.

## Concepts
There are some basic concepts to know about the viewer.

- World
- World items
- Spacial content
- Runtime + Scheduler
- Event Dispatcher
- Viewport

## Modularity
The viewer is made up of many individual modules that can be composed together or used in a pre-build bundle, more like a traditional viewer. There are currently 4 types of module that be used or created, each with a single responsibility.

### Data-source
The first thing that a viewer needs to do is to query a data source and load in some images into the World. A data source may make HTTP queries to fetch IIIF content, or maybe take static images. However it loads the data, it will yield abstract items – world objects that haven't been placed in a world yet. These items can be passed to a builder. Data-sources also add an ordering to their items.

- Hyperion data source
- Manifesto data source
- Image service data source [todo - repo]

### Builders
Builders take abstract items from one or more data sources and an empty World and arrange the objects in the world. Some builders may have additional configuration, like viewing direction or columns and rows in a grid. A builder can either run once in an application at the beginning, or be dynamic where items can be added or removed and the layout recalculated. This is optional though in builders and may not be supported by every one.

- Book builder
- Grid builder
- Minimap builder
- Scroll builder
- Flexbox builder [todo - repo]

### Renderers
Renders work in conjunction with the main runtime of Atlas and each frame will render out the images at the position they are in the world. The runtime will make sure that the positions are relative to the viewport and only the images that are visible are passed to the renderer. The renderer is free to take these positions and paint them however it wants. The 2 main implementations are static HTML image tags inside of a div and the primary renderer, the HTML canvas renderer which is perfect for deep zoom images.

- A-Frame renderer [todo - repo]
- Canvas renderer
- Debug renderer
- HTML renderer
- React renderer
- Vue renderer
- WebGL renderer

### Controllers
The Viewport in Atlas is uniquely simple. It is made up of 5 numbers. An `X` and `Y` value for the position in the world, a `width` and `height` for to specifcy the zoom and region to be shown and a scale, which determines the resolution of the images displayed. These 5 numbers are efficiently stored in Javascript, and you can change them at any time. Any changes will be picked up in the next cycle of the renderer. The controllers job is to take some external input and manipulate those 5 numbers. A simple example may be binding the arrow keys to change the `X` and `Y` values, or converting drag and scroll events to pan and zoom. Since there are no limitations on how those 5 numbers are changed, you can do anything, like using the new [GamePad](https://developer.mozilla.org/en-US/docs/Web/API/Gamepad_API/Using_the_Gamepad_API) to control the world with an XBOX controller.

- A-Frame controller
- Popmotion controller
- Gamepad controller
- Keyboard controller [todo - repo]

Together these 4 modules can be composed together to create all sorts of viewing experiences.