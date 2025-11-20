# AI World Gen – Development To-Do List

## Project Setup and Environment
- [ ] Install Python 3.12.3 and create venv: `python -m venv env; source env/bin/activate`
- [ ] Install libraries: `pip install pygame==2.6.0 pyopengl==3.1.7 moderngl==5.10.0 numpy==2.1.2 pillow==11.1.0 torch==2.5.0 diffusers==0.30.2`
- [ ] Verify GPU: Run `import torch; print(torch.cuda.is_available())`. If False, warn: "AI will be slow; use smaller resolutions."
- [ ] Directory Structure: See below.
- [ ] Git Init: `git init; git add .; git commit -m "Initial setup"`.
- [ ] Test: Run empty `main.py` with `print("Setup OK")`.

## Engine Core Development
- [ ] Initialize Window and OpenGL Context
- [ ] Rendering System Basics
- [ ] Input Handling and Camera Control
- [ ] Game World Structure
- [ ] Engine Optimization & Housekeeping

## AI Module Development
- [ ] Loading AI Models
- [ ] Command Syntax and Parser
- [ ] Generation Integration
- [ ] Modification Integration
- [ ] Prompt Engineering & Quality

## Putting It All Together
- [ ] Main loop: input, parse, queue AI, apply results, render.
- [ ] Test End-to-End: Run demo commands; screenshot for validation.

## Edge Cases & Validation
- [ ] Handle dup names, errors, demo script.

## Documentation and Final Touches
- [ ] README, comments, final test.

---

### Directory Structure
```
ai-world-gen/
├── main.py
├── engine/
│   ├── __init__.py
│   ├── renderer.py
│   ├── input.py
│   ├── world.py
│   └── loop.py
├── ai/
│   ├── __init__.py
│   ├── parser.py
│   ├── generator.py
│   └── assets.py
├── assets/
├── requirements.txt
├── todo.md
└── README.md
```
