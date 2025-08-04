---
title: "git Install"
weight: 11
pre: "0.2. "
---

## Verify a git install

You will likely already have a command-line version of `git` installed on your machine. To check, open a folder in VS Code, display the integrated terminal, and type:

```text
git --version
```

You should see a version number printing out. If you see that, `git` is already installed.

If you see an error that `git` is unrecognized, then you will need to install it. Go [here](https://git-scm.com/downloads) to download and install the latest version.

Windows users may need to add the `git.exe` location to the system `Path` environment variables. Most likely, `git.exe` will be installed to `C:\Program Files\Git\bin`. Check this location, copy its address, and type "Environment variables" in the Windows search. Click "Environment Variables" and find "Path" under System variables. Click "Edit...". Verify that `C:\Program Files\Git\bin` (or whatever your `git` location) is the last item listed. If it isn't, add a new entry for `C:\Program Files\Git\bin`.