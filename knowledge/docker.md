# Docker


## Python dependency management / requirements.txt
- **Best practice**: fully pinned dependencies auto-generated from a higher-level specification
- use `pip-compile`:
    - takes a high-level dependency file (usually `requirements.in` or `pyproject.toml`)
    - collects all available versions of each package from the package index (typically PyPI)
    - builds full reproducible dependency graph (including sub-dependencies), resolves version conflicts, selects the latest versions that satisfy all constraints
    - produces a fully pinned `requirements.txt` file, includes comments showing which top-level package caused which subdependency
    - Note: 
        - it does NOT depend on what versions are currently installed in your environment (unless you force it to)
        - if you add a package to an existing list in `requirements.in` and then RErun `pip-compile requirements.in`, all currently pinned versions in `requirements.txt` remain the same if they still satisfy all constraints (no uncontrolled upgrades)
        - controlled upgrades of all packages: `pip-compile --upgrade`
        - more granular upgrade of one package at a time: `pip-compile --upgrade-package <package_name>`
        - `pip-compile==7.5.2` and` pip==26.0.1` are incompatible (`AttributeError` with `'PackageFinder'`) => use `pip==25.3` instead.

## Local testing
- change to app repository where the Dockerfile is
- `docker build -t flask-api .`
- `docker run -p 8080:5000 flask-api`
    - HOST_PORT:CONTAINER_PORT
        - `HOST_PORT`: the mort you access on your machine with localhost:8080
        - `CONTAINER_PORT`: must match the port in the Dockerfile CMD command which is the port where the app accepts traffic

