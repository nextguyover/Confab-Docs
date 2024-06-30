# Confab Docs

### Documentation for Confab Comments, hosted at [docs.confabcomments.com](https://docs.confabcomments.com). See the [main Confab repo](https://github.com/nextguyover/Confab) for more information about this project.

# Setting Up Dev Environment

Install Python, then install mkdocs-material and required plugins using the following command:

```
pip install mkdocs-material=="9.*" mkdocs-awesome-pages-plugin mkdocs-markdownextradata-plugin
```

Make sure that `mkdocs` is accessible from the terminal, `mkdocs.exe` should be stored in a directory in PATH. `pip install` command should tell you whether the directory needs to be added to PATH.

# Development and Building

`mkdocs serve` to start the local server during development.

`mkdocs build` to build the site.