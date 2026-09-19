# XEN-GEN1-T-Code

XEN-GEN1-T-Code is the coding-specialized model in the XEN family.

It is designed for:
- Programming and code generation
- Debugging and error analysis
- Roblox Luau / scripting workflows
- Technical reasoning
- Mathematics

The repository includes built-in `SKILL.md` instructions that are loaded at inference time. Skills improve the workflow and task-specific behavior without changing the model weights.

## Supported platforms

- Windows
- Linux
- macOS

## Install

### Windows

```powershell
python -m venv .venv
.venv\\Scripts\\activate
pip install -r requirements.txt
```

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

If your system uses `python` for Python 3, you can use `python` instead of `python3`.

## Use the AI

First, make sure you have a trained model checkpoint at:

```
outputs/xen/model.pt
outputs/xen/tokenizer.json
```

Then run:

```bash
python inference/generate.py --model-dir outputs/xen --prompt "Write a Python function that checks whether a number is prime."
```

For a longer answer:

```bash
python inference/generate.py --model-dir outputs/xen --prompt "Explain this Python error and show how to fix it." --max-new-tokens 256
```

The script automatically uses CUDA when an NVIDIA GPU is available and falls back to CPU otherwise.

### Disable built-in skills

By default, the model loads the repository's `skills/` directory. To run without those inference-time skills:

```bash
python inference/generate.py --model-dir outputs/xen --prompt "Hello" --no-skills
```

### Use a different skills directory

```bash
python inference/generate.py --model-dir outputs/xen --prompt "Debug this code" --skills-dir ./my-skills
```

## Train the model

Create a JSONL dataset such as `datasets/train.jsonl`:

```json
{"instruction":"Write a Python function that adds two numbers.","response":"def add(a, b):\\n    return a + b"}
{"instruction":"Explain what a variable is in programming.","response":"A variable stores a value that a program can read or change."}
```

Then train:

```bash
python training/train.py --data datasets/train.jsonl --output outputs/xen --steps 1000
```

Training a specialized checkpoint can improve coding capability beyond the built-in skills.

## Repository structure

- `model/` — XEN-GEN1-T-Code architecture, tokenizer, loader, and skills loader.
- `training/` — training code.
- `inference/` — local generation.
- `skills/` — coding, debugging, and Roblox Luau skill instructions.
- `configs/system_prompt.txt` — model behavior and system instructions.

## Important note

XEN-GEN1-T-Code is a research/development model trained from scratch. The built-in skills are an inference-time instruction layer; they do not magically add knowledge to the model. For stronger capability, train the model on a high-quality coding dataset.

## License

MIT
