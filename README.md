# Threejs

## Table of Content

1. [Basic Scene](basic-scene)

## Basic Scene

We need 4 elements to get started:

- A scene that will contain objects
- Some objects
- A camera
- A render

### Scene

- Like a container
- We put objects, models, lights, etc. In it
- At some point we ask Three.js to render that scene

### Objects

Can be many things

- Primitive geometries
- Imported models
- Particles
- Lights
- Etc.

### [Mesh](https://threejs.org/docs/index.html?q=Mesh#api/en/objects/Mesh)

A mesh is the combination of a **geometry** (the shape) and a **material** (how
it looks). Start with a **BoxGeometry** and a **MeshBasicMaterial**.

### Camera

- Not visible
- Serve as point of view when doing a render
- Can have multiple and switch between them
- Different types
- We are going to use **PerspectiveCamera**

The first parameter is: **The Field of View**

- Vertical vision angle
- In degrees
- Also called **fov**

The second parameter is: **The Aspect Ratio**

The width of the render divided by the height of the render.

### Renderer

- Render the scene from the camera point of view
- Result drawn into a canvas
- A canvas is a HTML element in which you can draw stuff
- Three.js will use WebGL to draw the render inside this canvas
- You _can create it or you_ can let Three.js do it

<details>
  <summary>Here's the necessary code to make a red cube</summary>

```javascript
// Scene
const scene = new THREE.Scene();

// Red Cube
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);

// Sizes
const sizes = {
  width: 800,
  height: 600,
};

// Camera
const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height);
camera.position.z = 3;
camera.position.x = 1.5;
scene.add(camera);

// Renderer
const canvas = document.querySelector(".webgl");
const renderer = new THREE.WebGLRenderer({
  canvas: canvas,
});
renderer.setSize(sizes.width, sizes.height);

renderer.render(scene, camera);
```

</details>
