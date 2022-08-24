# Environment Modifier

This utility acts similar to the coreutils `env` application (`/usr/bin/env`),
but with one added feature: when running an application or script that resides
in a known directory, it can replace the application with a known alternative.

For example, say you have the following directory structure:
```
project/
  code/
    script.py  <-- Python script starting with: #!/usr/bin/env python3
  venv/        <-- Python virtual environment
```

By default, if you run `./project/code/script.py` on the command line, it will
execute with the system Python installation found by `/usr/bin/env python3`
(typically `/usr/bin/python3`).

Instead, we can define a file `project/.envmod` with the following:
```
python3=venv/bin/python3
```

Now, when we run `script.py`, it will use the virtual environment instead.

## Installation

1. Clone this repository.
2. Install the wrapper in place of `/usr/bin/env`:
   ```
   sudo ./envmod --install
   ```
3. Create a `.envmod` file as described below.

## `.envmod` File

The `.envmod` file is used to specify alternative applications to be
executed. Each line in the file contains a pattern structured as follows:

```
application=path/to/alternative
```

where `application` is the name of the program specified to `/usr/bin/env` that
should be replaced. `path/to/alternative` should be either an absolute path, or
a path relative to the parent directory where `.envmod` is located.

By default, config files are loaded in the following order of precedence:
1. `.envmod` located in the same directory as the script being run (see below), or in any parent directory, with
   patterns from higher level directories being overridden by those in lower level directories.
2. `${HOME}/.envmod`
3. `/etc/envmod`
