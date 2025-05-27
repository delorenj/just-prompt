# PyPI Upload Instructions for just-prompt

The package has been successfully built and is ready to upload! Here's how to complete the publishing process:

## Files Ready for Upload
- `dist/just_prompt-1.0.0-py3-none-any.whl` (46.7 KB)
- `dist/just_prompt-1.0.0.tar.gz` (33.2 KB)

## Option 1: Using PyPI API Token (Recommended)

1. **Get your PyPI API Token:**
   - Log in to https://pypi.org
   - Go to Account Settings → API tokens
   - Create a new token (scope it to this project after first upload)
   - Copy the token (starts with `pypi-`)

2. **Upload using the token:**
   ```bash
   cd /home/delorenj/code/just-prompt
   uv run twine upload dist/* -u __token__ -p YOUR_PYPI_TOKEN
   ```

## Option 2: Using Environment Variable

1. **Set the token as an environment variable:**
   ```bash
   export TWINE_USERNAME=__token__
   export TWINE_PASSWORD=YOUR_PYPI_TOKEN
   ```

2. **Then upload:**
   ```bash
   cd /home/delorenj/code/just-prompt
   uv run twine upload dist/*
   ```

## Option 3: Create .pypirc File

1. **Create `~/.pypirc` with your credentials:**
   ```ini
   [pypi]
   username = __token__
   password = YOUR_PYPI_TOKEN
   ```

2. **Secure the file:**
   ```bash
   chmod 600 ~/.pypirc
   ```

3. **Then upload:**
   ```bash
   cd /home/delorenj/code/just-prompt
   uv run twine upload dist/*
   ```

## After Successful Upload

Once uploaded, your package will be available at:
- https://pypi.org/project/just-prompt/

Users will be able to install it with:
```bash
pip install just-prompt
```

## Future Uploads

For future versions:
1. Update the version in `pyproject.toml`
2. Clean old builds: `rm -rf dist/ build/`
3. Build: `python -m build`
4. Upload: `uv run twine upload dist/*`

## Add to mise.toml

You might want to add these tasks to your mise.toml:
```toml
[tasks.build]
description = "Build distribution packages"
run = "rm -rf dist/ build/ && python -m build"

[tasks.publish]
description = "Publish to PyPI"
run = "uv run twine upload dist/*"
```