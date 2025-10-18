# 3D Gravity Simulation with Sphere Rendering - Complete Checklist

## Phase 1: Project Setup & Headers

- [x] Include GLEW for OpenGL extensions: `#include <GL/glew.h>`
- [x] Include GLFW for window management: `#include <GLFW/glfw3.h>`
- [x] Include GLM core: `#include <glm/glm.hpp>`
- [x] Include GLM matrix transformations: `#include <glm/gtc/matrix_transform.hpp>`
- [x] Include GLM type pointer utilities: `#include <glm/gtc/type_ptr.hpp>`
- [x] Include vector and iostream for data structures

## Phase 2: Vertex and Fragment Shaders

- [x] Write vertex shader with version `#version 330 core`
- [x] Add `layout(location=0) in vec3 aPos` for vertex position input
- [x] Add uniform variables: `mat4 model`, `mat4 view`, `mat4 projection`
- [x] In vertex shader: calculate `gl_Position = projection * view * model * vec4(aPos, 1.0)`
- [x] Write fragment shader with `#version 330 core`
- [x] Add `out vec4 FragColor` in fragment shader
- [x] Add `uniform vec4 objectColor` for dynamic coloring
- [x] Set `FragColor = objectColor` in fragment shader main function

## Phase 3: Global Variables & Camera Setup

- [x] Create camera position vector: `glm::vec3 cameraPos = glm::vec3(0.0f, 0.0f, 1.0f)`
- [x] Create camera front vector: `glm::vec3 cameraFront = glm::vec3(0.0f, 0.0f, -1.0f)`
- [x] Create camera up vector: `glm::vec3 cameraUp = glm::vec3(0.0f, 1.0f, 0.0f)`
- [x] Add mouse tracking variables: `lastX`, `lastY`, `yaw`, `pitch`
- [x] Add timing variables: `deltaTime`, `lastFrame`
- [x] Define physics constants: `G = 6.6743e-11`, `c = 299792458.0`

## Phase 4: Helper Function Declarations

- [x] Declare `GLFWwindow* StartGLU()` for window initialization
- [x] Declare `GLuint CreateShaderProgram(const char* vertexSource, const char* fragmentSource)`
- [x] Declare `CreateVBOVAO()` for buffer management
- [x] Declare `UpdateCam()` for camera matrix updates
- [x] Declare callback functions: `keyCallback`, `mouseButtonCallback`, `mouse_callback`, `scroll_callback`
- [x] Declare `glm::vec3 sphericalToCartesian(float r, float theta, float phi)`
- [x] Declare `DrawGrid()` and `CreateGridVertices()` for grid rendering

## Phase 5: Object Class - Structure

- [x] Create `Object` class with public members
- [x] Add GLuint members: `VAO`, `VBO` for OpenGL buffers
- [x] Add `glm::vec3 position` (default: `glm::vec3(400, 300, 0)`)
- [x] Add `glm::vec3 velocity` (default: `glm::vec3(0, 0, 0)`)
- [x] Add `size_t vertexCount` to track number of vertices
- [x] Add `glm::vec4 color` for object color (default: red)
- [x] Add bool flags: `Initializing`, `Launched`, `target`
- [x] Add physics properties: `float mass`, `float density`, `float radius`
- [x] Add `glm::vec3 LastPos` for tracking previous position

## Phase 6: Object Class - Constructor

- [x] Create constructor: `Object(glm::vec3 initPosition, glm::vec3 initVelocity, float mass, float density = 3344)`
- [x] Set position, velocity, mass, density from parameters
- [x] Calculate radius using formula: `pow(((3 * mass/density)/(4 * π)), (1.0/3.0)) / 100000`
- [x] Call `Draw()` to generate sphere vertices
- [x] Store vertex count: `vertexCount = vertices.size()`
- [x] Call `CreateVBOVAO(VAO, VBO, vertices.data(), vertexCount)`

## Phase 7: Sphere Generation - Draw() Method

- [x] Create `std::vector<float> Draw()` method
- [x] Initialize empty vertices vector
- [x] Set number of stacks (latitude divisions): `int stacks = 10`
- [x] Set number of sectors (longitude divisions): `int sectors = 10`
- [x] Create outer loop: `for(float i = 0.0f; i <= stacks; ++i)`
- [x] Calculate theta1: `(i / stacks) * glm::pi<float>()`
- [x] Calculate theta2: `((i+1) / stacks) * glm::pi<float>()`
- [x] Create inner loop: `for (float j = 0.0f; j < sectors; ++j)`
- [x] Calculate phi1: `(j / sectors) * 2 * glm::pi<float>()`
- [x] Calculate phi2: `((j+1) / sectors) * 2 * glm::pi<float>()`
- [x] Calculate 4 vertices: `v1, v2, v3, v4` using sphericalToCartesian()
- [x] Add first triangle vertices (v1, v2, v3) to vertices vector
- [x] Add second triangle vertices (v2, v4, v3) to vertices vector
- [x] Return vertices vector

## Phase 8: Sphere Coordinate Conversion

- [x] Implement `sphericalToCartesian(float r, float theta, float phi)`
- [x] Calculate x: `r * sin(theta) * cos(phi)`
- [x] Calculate y: `r * cos(theta)`
- [x] Calculate z: `r * sin(theta) * sin(phi)`
- [x] Return `glm::vec3(x, y, z)`

## Phase 9: Object Class - Physics Methods

- [x] Create `UpdatePos()` method
- [x] Update x position: `position[0] += velocity[0] / 94`
- [x] Update y position: `position[1] += velocity[1] / 94`
- [x] Update z position: `position[2] += velocity[2] / 94`
- [x] Recalculate radius based on current mass
- [x] Create `UpdateVertices()` method to regenerate sphere with new radius
- [x] Bind VBO and update with `glBufferData()`
- [x] Create `GetPos()` const method returning position vector
- [x] Create `accelerate(float x, float y, float z)` method
- [x] Apply acceleration: `velocity += acceleration / 96`

## Phase 10: Object Class - Collision Detection

- [x] Create `CheckCollision(const Object& other)` method
- [x] Calculate dx, dy, dz between objects
- [x] Calculate distance: `sqrt(dx*dx + dy*dy + dz*dz)`
- [x] Check if `other.radius + this->radius > distance`
- [x] Return -0.2f if collision detected (damping factor)
- [x] Return 1.0f if no collision

## Phase 11: Window Initialization - StartGLU()

- [x] Initialize GLFW with `glfwInit()`
- [x] Check initialization success, return nullptr if failed
- [x] Create window: `glfwCreateWindow(800, 600, "3D_TEST", NULL, NULL)`
- [x] Check window creation, terminate GLFW if failed
- [x] Make context current: `glfwMakeContextCurrent(window)`
- [x] Set `glewExperimental = GL_TRUE`
- [x] Initialize GLEW with `glewInit()`
- [x] Check GLEW initialization
- [x] Enable depth testing: `glEnable(GL_DEPTH_TEST)`
- [x] Set viewport: `glViewport(0, 0, 800, 600)`
- [x] Enable blending: `glEnable(GL_BLEND)`
- [x] Set blend function: `glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA)`
- [x] Return window pointer

## Phase 12: Shader Compilation - CreateShaderProgram()

- [x] Create vertex shader: `glCreateShader(GL_VERTEX_SHADER)`
- [x] Set shader source: `glShaderSource(vertexShader, 1, &vertexSource, nullptr)`
- [x] Compile vertex shader: `glCompileShader(vertexShader)`
- [x] Check compilation status with `glGetShaderiv(GL_COMPILE_STATUS)`
- [x] If failed, get error log with `glGetShaderInfoLog()` and print
- [x] Create fragment shader: `glCreateShader(GL_FRAGMENT_SHADER)`
- [x] Set fragment shader source and compile
- [x] Check fragment shader compilation status
- [x] Create shader program: `glCreateProgram()`
- [x] Attach both shaders: `glAttachShader()` twice
- [x] Link program: `glLinkProgram(shaderProgram)`
- [x] Check linking status with `glGetProgramiv(GL_LINK_STATUS)`
- [x] Delete individual shaders: `glDeleteShader()` for both
- [x] Return shader program ID

## Phase 13: Buffer Creation - CreateVBOVAO()

- [x] Generate VAO: `glGenVertexArrays(1, &VAO)`
- [x] Generate VBO: `glGenBuffers(1, &VBO)`
- [x] Bind VAO: `glBindVertexArray(VAO)`
- [x] Bind VBO: `glBindBuffer(GL_ARRAY_BUFFER, VBO)`
- [x] Fill VBO with data: `glBufferData(GL_ARRAY_BUFFER, vertexCount * sizeof(float), vertices, GL_STATIC_DRAW)`
- [x] Set vertex attribute pointer: `glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0)`
- [x] Enable vertex attribute: `glEnableVertexAttribArray(0)`
- [x] Unbind VAO: `glBindVertexArray(0)`

## Phase 14: Camera Update - UpdateCam()

- [x] Use shader program: `glUseProgram(shaderProgram)`
- [x] Create view matrix: `glm::lookAt(cameraPos, cameraPos + cameraFront, cameraUp)`
- [x] Get view uniform location: `glGetUniformLocation(shaderProgram, "view")`
- [x] Set view matrix uniform: `glUniformMatrix4fv(viewLoc, 1, GL_FALSE, glm::value_ptr(view))`

## Phase 15: Input Callbacks - Keyboard

- [x] Implement `keyCallback(GLFWwindow* window, int key, int scancode, int action, int mods)`
- [x] Calculate camera speed: `cameraSpeed = 1000.0f * deltaTime`
- [x] Handle W key: move camera forward (`cameraPos += cameraSpeed * cameraFront`)
- [x] Handle S key: move camera backward
- [x] Handle A key: move camera left (cross product)
- [x] Handle D key: move camera right (cross product)
- [x] Handle SPACE: move camera up
- [x] Handle LEFT_SHIFT: move camera down
- [x] Handle K key: pause/unpause simulation
- [x] Handle Q key: quit application
- [x] Handle arrow keys for object initialization positioning
- [x] Check SHIFT modifier for Z-axis movement during initialization

## Phase 16: Input Callbacks - Mouse

- [x] Implement `mouse_callback(GLFWwindow* window, double xpos, double ypos)`
- [x] Calculate mouse offset: `xoffset = xpos - lastX`, `yoffset = lastY - ypos`
- [x] Update lastX and lastY
- [x] Apply sensitivity: multiply offsets by 0.1f
- [x] Update yaw and pitch
- [x] Clamp pitch between -89 and 89 degrees
- [x] Calculate front vector using spherical coordinates
- [x] Normalize and update cameraFront
- [x] Implement `mouseButtonCallback()` for object creation
- [x] On LEFT_MOUSE_BUTTON press: create new object with Initializing=true
- [x] On LEFT_MOUSE_BUTTON release: set Initializing=false, Launched=true
- [x] Implement `scroll_callback()` for camera zoom (move along cameraFront)

## Phase 17: Main Function - Setup

- [] Call `StartGLU()` to create window
- [ ] Create shader program with vertex and fragment shaders
- [ ] Get uniform locations: `modelLoc`, `objectColorLoc`
- [ ] Use shader program
- [ ] Set cursor position callback: `glfwSetCursorPosCallback(window, mouse_callback)`
- [ ] Set scroll callback: `glfwSetScrollCallback(window, scroll_callback)`
- [ ] Disable cursor: `glfwSetInputMode(window, GLFW_CURSOR, GLFW_CURSOR_DISABLED)`
- [ ] Create projection matrix: `glm::perspective(glm::radians(45.0f), 800.0f/600.0f, 0.1f, 750000.0f)`
- [ ] Get projection uniform location
- [ ] Set projection matrix uniform
- [ ] Set initial camera position: `glm::vec3(0.0f, 1000.0f, 5000.0f)`

## Phase 18: Main Function - Object Initialization

- [ ] Create objects vector with initial objects
- [ ] Create Moon object: position `(3844, 0, 0)`, velocity `(0, 0, 228)`, mass `7.34767309e22`, density `3344`
- [ ] Create Earth object: position `(0, 0, 0)`, velocity `(0, 0, 0)`, mass `5.97219e24`, density `5515`
- [ ] Generate grid vertices: `CreateGridVertices(100000.0f, 50, objs)`
- [ ] Create grid VAO/VBO
- [ ] Print Earth and Moon radii for verification

## Phase 19: Main Function - Render Loop Structure

- [ ] Create while loop: `while (!glfwWindowShouldClose(window) && running == true)`
- [ ] Calculate deltaTime: `currentFrame - lastFrame`
- [ ] Clear buffers: `glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)`
- [ ] Set keyboard and mouse callbacks
- [ ] Call `UpdateCam()` to update view matrix

## Phase 20: Main Function - Mass Adjustment During Initialization

- [ ] Check if last object is initializing
- [ ] If RIGHT_MOUSE_BUTTON pressed: increase mass by 1% per second
- [ ] Recalculate radius based on new mass
- [ ] Call `UpdateVertices()` to regenerate sphere

## Phase 21: Main Function - Grid Rendering

- [ ] Use shader program
- [ ] Set grid color to white with transparency: `glUniform4f(objectColorLoc, 1.0f, 1.0f, 1.0f, 0.25f)`
- [ ] Regenerate grid vertices: `CreateGridVertices(10000.0f, 50, objs)`
- [ ] Bind grid VBO and update data with `glBufferData(GL_DYNAMIC_DRAW)`
- [ ] Call `DrawGrid()` to render grid

## Phase 22: Main Function - Physics Calculations

- [ ] Loop through all objects
- [ ] Set object color uniform
- [ ] Create nested loop for gravitational interactions
- [ ] Skip if same object or either is initializing
- [ ] Calculate dx, dy, dz between objects
- [ ] Calculate distance: `sqrt(dx*dx + dy*dy + dz*dz)`
- [ ] Calculate direction vector (normalized)
- [ ] Convert distance to meters: `distance *= 1000`
- [ ] Calculate gravitational force: `G * m1 * m2 / distance²`
- [ ] Calculate acceleration: `force / mass`
- [ ] Apply acceleration if not paused
- [ ] Check for collisions and apply velocity damping

## Phase 23: Main Function - Object Updates & Rendering

- [ ] If object is initializing: update radius and vertices
- [ ] If not paused: call `UpdatePos()` for each object
- [ ] Create model matrix: `glm::mat4(1.0f)`
- [ ] Translate model matrix by object position
- [ ] Set model matrix uniform: `glUniformMatrix4fv()`
- [ ] Bind object VAO
- [ ] Draw object: `glDrawArrays(GL_TRIANGLES, 0, vertexCount / 3)`
- [ ] Swap buffers: `glfwSwapBuffers(window)`
- [ ] Poll events: `glfwPollEvents()`

## Phase 24: Grid Creation - CreateGridVertices()

- [ ] Create function with parameters: `float size, int divisions, const std::vector<Object>& objs`
- [ ] Create vertices vector
- [ ] Calculate step size: `size / divisions`
- [ ] Calculate half size: `size / 2.0f`
- [ ] Generate X-axis grid lines (nested loops for x, z coordinates)
- [ ] Generate Z-axis grid lines (nested loops for x, z coordinates)
- [ ] Add line segments: push 2 vertices per line (start and end points)

## Phase 25: Grid Distortion (Gravitational Curvature)

- [ ] Loop through all grid vertices (step by 3 for x,y,z)
- [ ] For each vertex, loop through all objects
- [ ] Calculate vector from vertex to object
- [ ] Calculate distance to object
- [ ] Convert distance to meters
- [ ] Calculate Schwarzschild radius: `rs = (2*G*mass)/(c*c)`
- [ ] Calculate curvature: `z = 2 * sqrt(rs*(distance_m - rs)) * 100.0f`
- [ ] Accumulate displacement
- [ ] Apply vertical displacement: `vertices[i+1] = vertexPos[1] / 15.0f - 3000.0f`
- [ ] Return modified vertices

## Phase 26: Grid Drawing - DrawGrid()

- [ ] Use shader program
- [ ] Create identity model matrix
- [ ] Get model uniform location
- [ ] Set model matrix uniform
- [ ] Bind grid VAO
- [ ] Set point size (if needed): `glPointSize(5.0f)`
- [ ] Draw grid: `glDrawArrays(GL_LINES, 0, vertexCount / 3)`
- [ ] Unbind VAO

## Phase 27: Cleanup & Termination

- [ ] Loop through all objects
- [ ] Delete each object's VAO: `glDeleteVertexArrays(1, &obj.VAO)`
- [ ] Delete each object's VBO: `glDeleteBuffers(1, &obj.VBO)`
- [ ] Delete grid VAO and VBO
- [ ] Delete shader program: `glDeleteProgram(shaderProgram)`
- [ ] Terminate GLFW: `glfwTerminate()`

## Phase 28: Testing & Refinement

- [ ] Test camera movement (WASD, Space, Shift)
- [ ] Test mouse look (yaw and pitch)
- [ ] Test object creation (left mouse button)
- [ ] Test mass adjustment (right mouse button during initialization)
- [ ] Test pause functionality (K key)
- [ ] Verify gravitational interactions between objects
- [ ] Check collision detection
- [ ] Verify grid distortion effect
- [ ] Adjust physics constants if needed
- [ ] Fine-tune camera speed and mouse sensitivity

---

**Total Tasks: 200+** organized into 28 phases
**Complexity Level: Advanced** (3D graphics + physics simulation)
