# AI World Gen

A reproducible, AI-powered 3D voxel game engine from scratch.

## Installation
1. Install Python 3.12.3
2. Create and activate a virtual environment:
   - Windows: `python -m venv env && .\env\Scripts\activate`
   - Linux/Mac: `python -m venv env && source env/bin/activate`
3. Install dependencies:
   - `pip install -r requirements.txt`

## Usage
- Run the engine: `python main.py`
- Use `/` to open the in-game console and type commands (see `todo.md` for syntax).

## Directory Structure
See `todo.md` for full structure and development plan.

## Troubleshooting
- If you see OpenGL errors, update your GPU drivers.
- If `torch.cuda.is_available()` is False, AI will run slowly (CPU fallback).

## License
MIT
