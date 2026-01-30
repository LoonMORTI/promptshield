# Publishing to PyPI

Complete guide to publish PromptShield to PyPI.

---

## Prerequisites

1. **PyPI Account**
   - Create account at https://pypi.org/account/register/
   - Verify email

2. **Install build tools**
   ```bash
   pip install --upgrade build twine
   ```

3. **Create API token**
   - Go to https://pypi.org/manage/account/token/
   - Create token with scope: "Entire account"
   - Save token securely

---

## Build Package

### 1. Clean previous builds
```bash
rm -rf dist/ build/ *.egg-info
```

### 2. Build distribution
```bash
python -m build
```

This creates:
- `dist/promptshield-2.0.0.tar.gz` (source)
- `dist/promptshield-2.0.0-py3-none-any.whl` (wheel)

### 3. Check package
```bash
twine check dist/*
```

---

## Test on TestPyPI (Recommended)

### 1. Upload to TestPyPI
```bash
twine upload --repository testpypi dist/*
```

Username: `__token__`
Password: `pypi-...` (your TestPyPI token)

### 2. Test install
```bash
pip install --index-url https://test.pypi.org/simple/ promptshield
```

### 3. Test import
```python
from promptshield import Shield
shield = Shield.balanced()
print("✅ Works!")
```

---

## Publish to PyPI

### 1. Double-check version
Ensure `pyproject.toml` has correct version:
```toml
version = "2.0.0"
```

### 2. Upload to PyPI
```bash
twine upload dist/*
```

Username: `__token__`
Password: `pypi-...` (your PyPI token)

### 3. Verify on PyPI
Visit: https://pypi.org/project/promptshield/

### 4. Test install
```bash
pip install promptshield
```

---

## Post-Release

### 1. Tag release on GitHub
```bash
git tag -a v2.0.0 -m "PromptShield v2.0.0 - Configurable Shield Architecture"
git push origin v2.0.0
```

### 2. Create GitHub Release
- Go to https://github.com/Neural-alchemy/promptshield/releases/new
- Select tag: `v2.0.0`
- Title: "v2.0.0 - Configurable Shield Architecture"
- Copy release notes from `walkthrough.md`
- Attach: `dist/promptshield-2.0.0.tar.gz`

### 3. Update README badge
```markdown
[![PyPI](https://img.shields.io/pypi/v/promptshield)](https://pypi.org/project/promptshield/)
```

---

## Quick Commands

```bash
# Clean, build, check
rm -rf dist/ build/ *.egg-info
python -m build
twine check dist/*

# Test on TestPyPI
twine upload --repository testpypi dist/*

# Publish to PyPI
twine upload dist/*

# Tag and release
git tag v2.0.0
git push origin v2.0.0
```

---

## Troubleshooting

### "File already exists"
- Version already published
- Increment version in `pyproject.toml`
- Rebuild: `python -m build`

### "Invalid credentials"
- Use `__token__` as username
- Use full token including `pypi-` prefix

### "Package not found after upload"
- Wait 1-2 minutes for PyPI indexing
- Check: https://pypi.org/project/promptshield/

---

## Version Incrementing

For future releases:

```toml
# pyproject.toml
version = "2.0.1"  # Patch: bug fixes
version = "2.1.0"  # Minor: new features
version = "3.0.0"  # Major: breaking changes
```

Then rebuild and re-upload.

---

## Security

**Never commit:**
- PyPI tokens
- API keys
- Private keys (`.pem`)

These should only exist in:
- Local environment variables
- GitHub Secrets (for CI/CD)
- Password managers

---

**Ready to publish!** Follow the steps above to get PromptShield on PyPI.
