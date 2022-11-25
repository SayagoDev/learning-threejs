#  Threejs

## Tabla de Contenido
1. [Basic Scene](basic-scene)

## Basic Scene

Necesitamos 4 elementos para iniciar:

- Una escena que contendrá todos los objetos
- Algunos objetos
- Una cámara
- Y un *renderer*

### Escena

La escena la podemos ver como un contenedor en donde podemos
agregar objectos, modelos, luces, etc. Y en algún punto de
nuestro código, le decimos a Three.js que renderice la escena.

### Objectos

Los objectos pueden ser varias cosas. Tenemos geometrías primitivas,
partículas, luces, así como importar modelos hechos en otros programas.

### Malla

Una malla es la combinación de la geometría de un objecto (su forma) y
un material (cómo luce).

### Cámara

La cámara no es visible, es más como un punto de vista teórico. Cuando
queremos renderizar una escena, está debe estar enfrente de nuestro
punto de vista.

Para crear una cámara, debemos usar la clase `PerspectiveCamera`. Hay
dos parámetros esenciales que debemos proporcionar:

- El campo de vista: El fov (field of view) es qué tan grande en su
ángulo de visión. Si se utiliza un ángulo muy grande, seremos capaces
de ver más en cada dirección. Pero esto puede distorsionar como vemos la
escena.
- La relación de aspecto: En la mayoría de casos la relación de aspecto
es la anchura del canvas dividido por su altura.

### Renderizador

El trabajo del renderizar es hacer el render de la escena. Simplemente
debemos decirle al renderizador que renderice la escena desde el punto
de vista de la cámara. Y el resultado se dibujara en el canvas.

<details>
  <summary>Aquí está todo el código necesario para crear un cubo rojo</summary>

  ```javascript
  // Scene
  const scene = new THREE.Scene()

  // Red Cube
  const geometry = new THREE.BoxGeometry(1, 1, 1)
  const material = new THREE.MeshBasicMaterial({ color: 0xff0000 })
  const mesh = new THREE.Mesh(geometry, material)
  scene.add(mesh)

  // Sizes
  const sizes = {
    width: 800,
    height: 600
  }

  // Camera
  const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height)
  camera.position.z = 3
  scene.add(camera)

  // Renderer
  const canvas = document.querySelector('.webgl')
  const renderer = new THREE.WebGLRenderer({
    canvas: canvas
  })
  renderer.setSize(sizes.width, sizes.height)

  renderer.render(scene, camera)
  ```
</details>
