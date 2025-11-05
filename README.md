# Reproduction: Python 3.14t Setup Failure on Windows ARM64

This repository demonstrates a bug in `actions/setup-python@v6` (and @v5) when trying to install Python 3.14t (free-threaded) on Windows ARM64 runners.

## The Bug

When using `actions/setup-python@v6` to install Python 3.14t on `windows-11-arm` runners, the action:

1. ✅ Successfully downloads `python-3.14.0-win32-arm64-freethreaded.zip` from GitHub releases
2. ✅ Successfully extracts the archive
3. ❌ **FAILS** because the extracted archive doesn't contain `python.exe` or `python3.14t.exe`

### Error Message

```
Remove-Item : Cannot find path 'C:\hostedtoolcache\windows\Python\3.14.0\arm64-freethreaded\python.exe' because it does not exist.
```

```
New-Item : Cannot find path 'C:\hostedtoolcache\windows\Python\3.14.0\arm64-freethreaded\python3.14t.exe' because it does not exist.
```

## Reproduction Steps

1. Fork this repository
2. Enable GitHub Actions
3. Push a commit or manually trigger the workflow
4. Observe the failure in the "Setup Python 3.14t (free-threaded)" step

## Environment

- **Runner**: `windows-11-arm`
- **Action**: `actions/setup-python@v6` (also fails with @v5)
- **Python Version**: `3.14t` (free-threaded)
- **Architecture**: `arm64`

## Expected Behavior

The action should successfully install Python 3.14t and allow running Python code with free-threading enabled.

## Actual Behavior

The action fails with errors about missing Python executables after extracting the downloaded archive.

## Related Issues

- The archive exists at: https://github.com/actions/python-versions/releases/download/3.14.0-18313368925/python-3.14.0-win32-arm64-freethreaded.zip
- The archive downloads successfully (HTTP 200)
- But the extracted contents appear to be incomplete or malformed

## Workaround

Remove `3.14t` from the python-version list when building for Windows ARM64.

## Where to Report

This should be reported to:
- https://github.com/actions/setup-python/issues
- https://github.com/actions/python-versions/issues
