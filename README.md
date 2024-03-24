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

## Transform Objects

There are 4 properties to transform objects:

- position
- scale
- rotation
- quaternion

### Move Objects

With `position` which has 3 properties:

- x
- y
- z

The direction of each axis is arbitrary in Three.js, we consider:

- `y` axis is going upward
- `z` axis is going backward
- `x` axis is going to the right/left

### [Axes Helper](https://threejs.org/docs/index.html?q=axes#api/en/helpers/AxesHelper)

Position things in space can be hard. One good solution is to use the AxeHelper
to display a colored line for each axis.

### Scale Objects

With `scale` which has 3 properties (Vector Three):

- x
- y
- z

### Rotate Objects

- With `rotation` or with `quaternion`
- Updating one will automatically update the other

**Rotation:**

- Also has `x`, `y` and `z` properties but it's a [Euler](https://threejs.org/docs/index.html?q=euler#api/en/math/Euler)
- When you change `x`, `y` and `z` properties you can imagine putting a stick
  through your object's center in the axis's direction and then rotating that
  object on that stick
- The value of these is expressed in radians
- Half a rotation is something like 3.14159... But you can use `Math.PI`
- When you rotate on an axis, you might also rotate the other axis
- The rotation goes by default in the `x`, `y` and `z` order and you can get
  strange result like an axis not working anymore, this is called _gimbal lock_

**Quaternion:**

- Also expressed a rotation, but in a more mathematical way
- `quaternion` updates when you change the `rotation`

Objects3D instances have a `lookAt(...)` method which rotated the object so that
it's `-z` faces the target you provided. The target must be a Vector3.

### Scene Graph

You can put objects inside groups and use `position`, `rotation` (or `quaternion`), and `scale` on those groups.

## Animations

### Request Animation Frame

The purpose of [requestAnimationFrame]() is to call the function provided on the
next frame. We are going to call the same function on each new frame.

### Adaptation to the Framerate

We need to know how much time it's been since the last tick. Use `Data.now()` to
get the current **timestamp.**

## Cameras

**Camera:**
[Camera](https://threejs.org/docs/index.html?q=Camera#api/en/cameras/Camera) is an abstract class. You're not suppose to use it directly.

**Array Camera:**
Render the scene from multiple cameras on specifics areas of the
render.

**Stereo Camera:**
Render the scene through two cameras that mimic the eyes to create a parallax
effect. Use with devices like VR headset, red and blue glasses or cardboard.

**Cube Camera:**
Do 6 renders, each one facing a different direction. Can render the surrounding
for things like environment map, reflection or shadow map.

**Orthographic Camera:**
Render the scene without perspective.

**Perspective Camera:**
Render the scene with perspective.

### Perspective Camera

Parameters:

- **Field of view:**

  - Vertical vision angle
  - In degrees
  - Also called fov

- **Aspect Ration:**

  - The width of the render divided by the height of the render

- **Near and Far:**
  - The third and fourth parameters called **near** and **far**, correspond to
    how close and how far the camera can see
  - Any object or part of the object closer than `near` or further than `far`
    will not show up
  - Do not use extreme values like `0.0001` and `999999` to prevent z-fighting

### Orthographic Camera

OrthographicCamera differs from PerspectiveCamera by it's lack of perspective.
Objects has the same size regardless of their distance to the camera.

**Parameters:**

Instead of a field of view, we provide how far the camera can see in each
direction (`left`, `right`, `top` and `bottom`), then the `near` and `far`.

### Custom Controls

We want to control the camera position with the mouse.

1. First we need the coordinates of the mouse on the page:

```javascript
const cursor = {
  x: 0,
  y: 0,
};
window.addEventListener("mousemove", (event) => {
  cursor.x = event.clientX / sizes.width - 0.5;
  cursor.y = -(event.clientY / sizes.height - 0.5);
});
```

2. Then, we need to update the camera in the `tick` function.

```javascript
const tick = () => {
...

  // Update camera
  camera.position.x = Math.sin(cursor.x * Math.PI * 2) * 3;
  camera.position.y = cursor.y * 5;
  camera.position.z = Math.cos(cursor.x * Math.PI * 2) * 3;
  camera.lookAt(mesh.position);

...
}
```

- By using the `Math.PI * 2`, we can set a perfect 360 degree turn, in the x and z
  axis.

### Built-in Controls

**FlyControls:**
Enable moving the camera like if you were on a spaceship. You can rotate on all
3 axes, go forward and go backward.

**First Person Controls:**
Is like FlyControls, but with a fixed up axis. Doesn't work like in "FPS"
games.

**Pointer Lock Controls:**
Uses the pointer lock JavaScript API. Hard to use and almost only handles the
pointer lock and camera rotation.

**Orbit Controls:**
Similar to the controls we made with move features.

**Trackball Controls:**
Is like OrbitControls without the vertical angle limit.

**Transform Controls:**
Has nothing to do with the camera.

**Drag Controls:**
Has nothing to do with the camera.

## Fullscreen and resizing

### Fit in the Viewport

To get the viewport width and height, use `window.innerWidth` and
`window.innerHeight`

### Handle Resize

We need to update the sizes, the camera and the renderer. Using an
`addEventListener`.

```javascript
window.addEventListener("resize", () => {
  // Update sizes
  sizes.width = window.innerWidth;
  sizes.height = window.innerHeight;

  // Update camera
  camera.aspect = sizes.width / sizes.height;
  camera.updateProjectionMatrix();

  // Update renderer
  renderer.setSize(sizes.width, sizes.height);
});
```

### Handle Pixel Ratio

Some might see a blurry render and stairs effect on the edges. If so, it's
because you are testing on a screen with a pixel ratio greater than `1`.

- The **pixel ratio** corresponds to how many physical pixels you have on the
  screen for one pixel unit on the software part
- Highest pixel ratios are usually on the weakest devices —mobiles
- To get the current pixel ratio, we can use `window.devicePixelRatio`
- To update the `renderer` accordingly, we can use `renderer.setPixelRatio(...)`

### Handle Fullscreen

Let's add support to a fullscreen mode by double cliking anywhere. Listen to the
`dbclick` event.

## Geometry

- Composed of **vertices** (point coordinates in 3D spaces) and **faces**
  (triangle's that join those vertices to create a surface)
- Can be used for meshes but also for particles
- Can store more data than the positions (UV coordinates, normals, colors, or
  anything we want)

### Built-in Geometries

- BoxGeometry
- PlaneGeometry
- CircleGeometry
- ConeGeometry
- CylinderGeometry
- RingGeometry
- TorusGeometry
- TorusKnotGeometry
- DodecahedronGeometry
- TextGeometry
- ... And much more.

#### BoxGeometry

**6 Parameters:**

- `width`: The size on the `x` axis
- `height`: The size on the `y` axis
- `depth`: The size on the `z` axis
- `widthSegmets`: How many subdivisions in the `x` axis
- `heightSegments`: How many subdivisions in the `y` axis
- `depthSegments`: How many subdivisions in the `z` axis

Subdivisions correspond to how much triangles should compose a face:

- `1` = 2 triangles per face
- `2` = 8 triangles per face

### Creating Your Own Buffer Geometry

Before creating the geometry, we need understand how to store buffer geometry
data. We are going to use Float32Array.

- Typed array
- Can only store floats
- Fixed length
- Easier to handle for the computer

## Debug UI

There are different types of elements you can add to that panel:

- **Range:** for numbers with minimum and maximum value
- **Color:** for colors with various formats
- **Text:** for simple texts
- **Checkbox:** for booleans (`true` or `false`)
- **Select:** for a choice from a list of values
- **Button:** to trigger functions
- **Folder:** to organize your panel if you have too many elements (tweaks)

### Colors

## Textures

Textures are images that will cover the surface of the geometries. Many types
with many different effects.

More use types of textures:

**Color:** 
- Most simple one
- Applied on the geometry

**Alpha:** 
- Grayscale image
- White visible
- Black not visible

**Height (or displacement):** 
- Grayscale image
- Move the vertices to create some relief
- Need enough subdivionsR

**Normal:** 
- Add details
- Doesn't need subdivision
- The vertices won't move
- Lure the light about the face orientation
- Better performance than adding a height text with a lot of subdivision

**Ambient Occlusion:** 
- Grayscale image
- Add fake shadows in crevices
- Not physically accurate
- Helps to create constrast and see details

**Metalness:** 
- Grayscale image
- White is metallic
- Black is non-metallic
- Mostly for reflection

**Roughness:** 
- Grayscale image
- In duo with the metalness
- White is rough
- Black is smooth
- Mostly for light dissipation

Those textures (especially the metalness and the roughness) follow the PBR
principles:

- Physically Based Rendering
- Many technics that tend to follow real-life directions to get realistic
results
- Becoming the standard for realistic renders

### UV Unwrapping
