# WebGL

WebGL is a rasterization API.

**clip space**: `{ (x, y) | -1 <= x <= 1, -1 <= y <= 1 }`

**shader**: a program that runs on the GPU.

**vertex shader**: calculates vertex positions. Ie, provides clip space coordinates. `gl_Position` is a `vec4` with x,y,z,w.

**fragment shader**: computes a color for each pixel of the primitive currently being drawn. `outColor` is a `vec4` of R,G,B,alpha.

**GL Shader Language (GLSL)**: the language shaders are written in.

**program**: a vertex shader and fragment shader paired together.

**attributes**: used to specify *how* to pull data out of buffer. Eg. which buffer, type of data, offset, how much.

**Vertex Array Object (VAO)**: a collection of attribute state. The state of attributes, which buffers to use for each one, and how to pull out data from those buffers is collected into a VAO.

Data used by the shaders must be provided to the GPU. There are 4 ways a shader can receive data.

1. Attributes, Buffers, and Vertex Arrays. (data pulled from buffers)
2. Uniforms: global variables. (values that stay the same for all vertices of a single draw call)
3. Textures: arrays of data that can be randomly accessed. (data from pixels/texels)
4. Varyings: way for a vertex shader to pass data to a fragment shader.

---

## `canvas` HTML tag

```html
<canvas id="canvas"></canvas>
```

```js
const canvas = document.querySelector("#canvas");
```

## Creating a program

Vertex shader

```glsl
#version 300 es

// an attribute is an input (in) to a vertex shader.
// It will receive data from a buffer
in vec4 a_position;

// all shaders have a main function
void main() {

  // gl_Position is a special variable a vertex shader
  // is responsible for setting
  gl_Position = a_position;
}
```

Fragment shader

```glsl
#version 300 es

// fragment shaders don't have a default precision so we need
// to pick one. highp is a good default. It means "high precision"
precision highp float;

// we need to declare an output for the fragment shader
out vec4 outColor;

void main() {
  // Just set the output to a constant reddish-purple
  outColor = vec4(1, 0, 0.5, 1);
}
```

Create

```js
const shader = gl.createShader(gl.VERTEX_SHADER); // gl.VERTEX_SHADER or gl.FRAGMENT_SHADER
gl.shaderSource(shader, sourceGLSLString);
gl.compileShader(shader);
```

Link

```js
const program = gl.createProgram();
gl.attachShader(program, vertexShader);
gl.attachShader(program, fragmentShader);
gl.linkProgram(program);
```

### Put data in buffer

Get location of attribute for the progam created

```js
const positionAttributeLocation = gl.getAttribLocation(program, "a_position");
```

Create a GPU buffer

```js
const positionBuffer = gl.createBuffer();
```

First you bind a resource to a bind point. Then, all other functions refer to the resource through the bind point.

```js
gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
```

copy data to the buffer on the GPU

```js
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([...]), gl.STATIC_DRAW);
```

**Note**: data gets sent to `positionBuffer` because it was bound to `gl.ARRAY_BUFFER`.

## Instruct attribute how to get data from GPU buffer

Attributes are used to specify how to pull data out of buffers and provide them to the vertex shader.

Create Vertex Array Object (VAO)

```js
const vao = gl.createVertexArray();
```

Make it the current so that attribute settings are apply to it

```js
gl.bindVertexArray(vao);
```

turn the VAO on with the desired Attribute Location from above, so that WebGL gets data from it

```js
gl.enableVertexAttribArray(positionAttributeLocation);
```

Instuct on how to pull data out of buffer

```js
const size = 2;          // 2 components per iteration
const type = gl.FLOAT;   // the data is 32bit floats
const normalize = false; // don't normalize the data
const stride = 0;        // 0 = move forward size * sizeof(type) each iteration to get the next position
const offset = 0;        // start at the beginning of the buffer
gl.vertexAttribPointer(positionAttributeLocation, size, type, normalize, stride, offset)
```

**Note**: A hidden part of `gl.vertexAttribPointer` is that it binds the current `ARRAY_BUFFER` to the attribute. In other words now this attribute is bound to `positionBuffer`. That means we’re free to bind something else to the `ARRAY_BUFFER` bind point. The attribute will continue to use `positionBuffer`.

Bind to inform which set of buffers to use and how to pull data out of those buffers to supply to the attributes

```js
gl.bindVertexArray(vao);
```

## Textures

Pass texture coordinates from the vertex shader to the fragment shader in a varying `v_texcoord`

```
in vec2 a_texcoord;
out vec2 v_texcoord;
void main() {
  v_texcoord = a_texcoord;
}
```

- In the fragment shader, declare a `uniform sampler2D` which lets us reference a texture.
- call `texture` to look up color from that texture.

```
in vec2 v_texcoord;
uniform sampler2D u_texture;
out vec4 outColor;
void main() {
  outColor = texture(u_texture, v_texcoord);
}
```

