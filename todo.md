# Building a 3D AI-Powered Game Engine from Scratch – Comprehensive, Perfected Roadmap

## 1. Environment and Dependency Setup (Atomic Steps)
    - Windows: `env\Scripts\activate`
    - Mac/Linux: `source env/bin/activate`
    ```
    pygame==2.6.0
    pyopengl==3.1.7
    moderngl==5.10.0
    numpy==2.1.2
    pillow==11.1.0
    torch==2.5.0
    diffusers==0.30.2
    ```
    ```python
    import torch
    print(torch.cuda.is_available())
    ```
    ```
    mkdir engine ai assets
    echo > engine/__init__.py
    echo > ai/__init__.py
    echo > engine/renderer.py
    echo > engine/input.py
    echo > engine/world.py
    echo > engine/loop.py
    echo > ai/parser.py
    echo > ai/generator.py
    echo > ai/assets.py
    echo > main.py
    echo > README.md
    ```
    ```python
    print("Setup OK")
    ```

## 1. Environment and Dependency Setup (Atomic Steps)
- [x] Download Python 3.12.3 installer from https://www.python.org/downloads/release/python-3123/
- [x] Run the installer. On Windows, check "Add Python to PATH".
- [x] Open a terminal and run `python --version` to confirm installation.
- [x] In terminal, run `mkdir ai-world-gen` to create the project folder.
- [x] `cd ai-world-gen`
- [x] Run `python -m venv env` to create a virtual environment.
- [x] Activate the environment:
        - Windows: `env\Scripts\activate`
        - Mac/Linux: `source env/bin/activate`
- [x] In the project root, create a file named `requirements.txt`.
- [x] Copy and paste the following into `requirements.txt`:
        ```
        pygame==2.6.0
        pyopengl==3.1.7
        moderngl==5.10.0
        numpy==2.1.2
        pillow==11.1.0
        torch==2.5.0
        diffusers==0.30.2
        ```
- [x] In terminal, run `pip install -r requirements.txt`.
- [ ] In Python REPL, run:
        ```python
        import torch
        print(torch.cuda.is_available())
        ```
- [ ] If output is `False`, add a warning to README: "AI will be slow; use smaller resolutions."
- [ ] In terminal, run `git init`.
- [ ] Run `git add .` and `git commit -m "Initial setup"`.
- [x] In terminal, run:
        ```
        mkdir engine ai assets
        echo > engine/__init__.py
        echo > ai/__init__.py
        echo > engine/renderer.py
        echo > engine/input.py
        echo > engine/world.py
        echo > engine/loop.py
        echo > ai/parser.py
        echo > ai/generator.py
        echo > ai/assets.py
        echo > main.py
        echo > README.md
        ```
- [x] In `main.py`, add:
        ```python
        print("Setup OK")
        ```
- [x] In terminal, run `python main.py` and confirm output is `Setup OK`.

## 2. Engine Core Development (Atomic Steps)
## 2. Engine Core Development (Atomic Steps)
### 2.1 Window and OpenGL Context
...existing code...
- [ ] In `engine/renderer.py`, add:
    ```python
    import pygame
    from pygame.locals import DOUBLEBUF, OPENGL
    from OpenGL.GL import *
    from OpenGL.GLU import *
    def init_window():
        pygame.init()
        display = (800, 600)
        pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
        gluPerspective(60, (display[0]/display[1]), 0.1, 1000.0)
        glEnable(GL_DEPTH_TEST)
    ```
- [ ] In `main.py`, call `from engine.renderer import init_window; init_window()`
- [ ] In `main.py`, add a loop:
    ```python
    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
        glClearColor(0.2, 0.3, 0.3, 1.0)
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        pygame.display.flip()
        pygame.time.wait(10)
    pygame.quit()
    ```
- [ ] Run and confirm window opens and closes cleanly.

### 2.2 Camera and View Matrix
- [ ] In `engine/renderer.py`, add:
    ```python
    import numpy as np
    class Camera:
        def __init__(self):
            self.pos = np.array([0, 5, 10], dtype=float)
            self.yaw = 0
            self.pitch = -20
        def get_view(self):
            # TODO: Implement full rotation matrix
            rot = np.eye(3)
            trans = np.eye(4)
            trans[:3, 3] = -self.pos
            return rot @ trans
    ```
- [ ] In `main.py`, instantiate Camera and print its position and view matrix.

### 2.3 Shaders and Test Cube
- [ ] In `engine/renderer.py`, add ModernGL shader strings for vertex and fragment shaders.
- [ ] Add code to compile shaders and create a program.
- [ ] Define cube vertices, indices, texcoords, normals as NumPy arrays.
- [ ] Create VBO and VAO for the cube.
- [ ] In render loop, set MVP matrix, bind texture, and render the cube.
- [ ] Run and confirm cube appears at origin and camera can rotate.

### 2.4 Input Handling and Camera Control
- [ ] In `engine/input.py`, add:
    ```python
    import pygame
    import numpy as np
    class InputHandler:
        def update(self, camera, dt):
            keys = pygame.key.get_pressed()
            forward = np.array([np.cos(camera.yaw), 0, -np.sin(camera.yaw)])
            if keys[pygame.K_w]: camera.pos += forward * dt * 5
            # Add S, A, D, space, ctrl similarly
    ```
- [ ] In `main.py`, instantiate InputHandler and call `update(camera, dt)` in the loop.
- [ ] Add mouse look: `pygame.mouse.set_visible(False); rel = pygame.mouse.get_rel(); camera.yaw -= rel[0]*0.1; camera.pitch -= rel[1]*0.1` (clamp pitch).
- [ ] Test: Move camera and rotate with mouse.

### 2.5 In-Game Console and Text Rendering
- [ ] In `main.py`, add logic to toggle console with '/'.
- [ ] Build command string from KEYDOWN events (alphanum, backspace, enter).
- [ ] Use Pygame font to render the command string as overlay after 3D draw.
- [ ] Test: Type in console and see text.

### 2.6 Voxel Grid and World Mesh
- [ ] In `engine/world.py`, add:
    ```python
    import numpy as np
    class World:
        def __init__(self, size=(64,16,64)):
            self.grid = np.zeros(size, dtype=np.uint8)
            self.grid[:,0,:] = 1
        def get_mesh(self):
            # TODO: Loop x,y,z; add faces if adjacent air
            verts = []; indices = []; texcoords = []
            return verts, indices, texcoords
    ```
- [ ] In renderer, call `World().get_mesh()` and render the mesh.
- [ ] Test: Render flat ground.

### 2.7 Texture Array and Block Types
- [ ] In renderer, create GL_TEXTURE_2D_ARRAY (128x128x16, RGBA).
- [ ] Implement function to update texture layers with images.
- [ ] Support per-face block type in mesh attributes.
- [ ] Test: Use placeholder textures for blocks.

### 2.8 Object List and Billboards
- [ ] In `engine/world.py`, add object list: each object has pos, texture_id, geometry ('billboard').
- [ ] Implement billboard rendering (quads always face camera).
- [ ] Test: Place a test object and verify rendering.

### 2.9 Save/Load System
- [ ] Implement save/load for world and objects using JSON.
- [ ] Save assets as PNG in `assets/`.
- [ ] Test: Save, reload, and verify world state.

### 2.10 Engine Optimization and Housekeeping
- [ ] Add FPS cap (60 FPS) using Pygame clock.
- [ ] Track GL resource IDs and clean up with glDeleteTextures on asset removal.
- [ ] Test: No resource leaks on asset removal.

## 3. AI Module Development (Atomic Steps)
### 3.1 AI Model Loading and Test
- [ ] In `ai/generator.py`, import `StableDiffusionPipeline` and `torch`.
- [ ] Load the pixel-art model:
    ```python
    from diffusers import StableDiffusionPipeline
    import torch
    pipe = StableDiffusionPipeline.from_pretrained("nerijs/pixel-art-xl", torch_dtype=torch.float16)
    pipe.to("cuda" if torch.cuda.is_available() else "cpu")
    # Optional: pipe.load_lora_weights("latent-consistency/lcm-lora-sdv1-5")
    ```
- [ ] Add a function to generate a 128x128 image from a prompt, using a fixed seed:
    ```python
    def generate_image(prompt):
        gen = torch.Generator("cuda" if torch.cuda.is_available() else "cpu").manual_seed(hash(prompt) % 2**32)
        image = pipe(prompt, width=128, height=128, generator=gen).images[0]
        return image
    ```
- [ ] Test: Call `generate_image("test prompt")` and save the result to `assets/test.png`.

### 3.2 AI Editing Model Loading
- [ ] In `ai/generator.py`, import `StableDiffusionInstructPix2PixPipeline`.
- [ ] Load the editing model:
    ```python
    from diffusers import StableDiffusionInstructPix2PixPipeline
    edit_pipe = StableDiffusionInstructPix2PixPipeline.from_pretrained("timbrooks/instruct-pix2pix", torch_dtype=torch.float16)
    edit_pipe.to("cuda" if torch.cuda.is_available() else "cpu")
    ```
- [ ] Add a function to edit an image with an instruction and fixed seed.
- [ ] Test: Edit a sample image and save the result.

### 3.3 Deterministic AI Generation
- [ ] Always use a prompt-hash-based seed for all AI generations and edits.
- [ ] Document this in code comments and README.
- [ ] Test: Regenerate the same prompt multiple times and confirm identical outputs.

### 3.4 Command Parser
- [ ] In `ai/parser.py`, import `shlex`.
- [ ] Write a `parse_command(s: str) -> dict` function supporting:
    - `/generate <type:texture|object> <name> '<prompt>'`
    - `/modify <name> '<instruction>'`
    - `/place <name> <x> <y> <z>`
    - `/list`
    - `/help`
- [ ] Add error messages for invalid syntax.
- [ ] Test: Parse sample commands and assert output.

### 3.5 Async AI Task Queue
- [ ] In `ai/generator.py`, import `ThreadPoolExecutor` from `concurrent.futures`.
- [ ] Create a single-worker executor for AI tasks.
- [ ] Add a function to submit generation/edit tasks and check for results in the main loop.
- [ ] Test: Queue a generation and confirm UI does not freeze.

### 3.6 Asset Generation Pipeline
- [ ] In `ai/generator.py`, implement `generate_asset(type, name, prompt)`:
    - Append style: `f"{prompt}, pixel art sprite, transparent background"`
    - Use fixed seed
    - Generate image
    - Post-process: remove background (set alpha=0 for background pixels)
    - Save image to `assets/{name}.png`
- [ ] Test: Generate a sample asset and verify file.

### 3.7 Asset Application and Update
- [ ] In renderer, add function to load PNG as GL texture and update texture array.
- [ ] If type is 'object', spawn billboard at camera front.
- [ ] Test: Add asset and see it appear in game.

### 3.8 Asset Modification Pipeline
- [ ] In `ai/generator.py`, implement `modify_asset(name, instruction)`:
    - Load image from `assets/{name}.png`
    - Call edit pipeline with instruction and fixed seed
    - Save and update GL texture
- [ ] Test: Modify an asset and see update in game.

### 3.9 Prompt Engineering and Quality
- [ ] Create prompt templates (e.g., `{'tree': 'a {desc} tree with branches, pixel art'}`)
- [ ] Add post-processing: quantize to 256 colors if retro look desired (use Pillow's `quantize`)
- [ ] On generation failure/artifacts, auto-regenerate with `+ "clean, no artifacts"` in prompt
- [ ] Test: Generate and refine assets for quality

## 4. Main Loop Integration and Testing (Atomic Steps)
### 4.1 Main Loop Integration
- [ ] In `main.py`, import all engine and ai modules.
- [ ] Set up the main game loop:
    - Poll for input events
    - Update camera and world state
    - Check AI task queue for results
    - Apply new assets or modifications
    - Render world, objects, and UI overlays
- [ ] Integrate command parser: when user submits a command, parse and dispatch to the correct AI or engine function.
- [ ] Test: Run the game, enter commands, and verify correct dispatch and feedback.

### 4.2 End-to-End Demo and Validation
- [ ] Prepare a demo script of at least 5 commands (e.g., generate, modify, place, list, help).
- [ ] Run the demo script step by step, verifying that each command produces the expected result in-game.
- [ ] Take screenshots after each step for documentation and reproducibility.
- [ ] Test: Re-run the demo script on a clean environment and confirm identical results (image hashes, world state).

## 5. Edge Cases, Error Handling, and Validation (Atomic Steps)
### 5.1 Edge Case Handling
- [ ] In command parser and asset manager, handle duplicate names by appending `_v2`, `_v3`, etc.
- [ ] Display clear error messages in the in-game console for invalid commands, missing assets, or failed AI generations.
- [ ] Add checks for GPU/CPU fallback and warn user if running on CPU.
- [ ] Test: Intentionally trigger errors and confirm correct handling and messaging.

### 5.2 Automated Validation
- [ ] Write a script to run the full demo sequence and compare output image hashes to reference values.
- [ ] Add regression tests for all major modules (parser, generator, renderer, etc.).
- [ ] Test: Run all tests and confirm pass/fail status.

## 6. Documentation, Code Quality, and Final Test (Atomic Steps)
### 6.1 Documentation
- [ ] Write a comprehensive `README.md` covering:
    - Project overview and goals
    - Full installation and setup instructions
    - How to run and use the engine
    - Command syntax and examples
    - Troubleshooting and FAQ
    - License and credits
- [ ] Add docstrings to all functions and classes (target: 80%+ coverage).
- [ ] Remove any outdated or redundant comments.

### 6.2 Final Test and Reproducibility
- [ ] On a clean machine, clone the repo and follow the README step by step.
- [ ] Run the full demo script and compare all outputs (images, world state) to reference values.
- [ ] Confirm that all steps are deterministic and reproducible.
- [ ] If any discrepancies are found, update documentation or code to resolve.

## 7. Future Improvements (Optional)
- [ ] Add support for custom LoRA fine-tuning for style consistency.
- [ ] Implement inpainting for local edits.
- [ ] Expand to procedural levels using noise libraries and AI textures.
- [ ] Add NPC AI using prompt-driven behavior trees.

---

This is now a maximally atomic, step-by-step, and foolproof project checklist. Every action, file, and test is a separate, explicit item.
