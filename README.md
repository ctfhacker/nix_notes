# Nix Notes

## Matthew Croughan - SCaLE 20x

Notes from (Matthew Croughan's talk)[https://www.youtube.com/watch?v=6Le0IbPRzOE]

### Creating different Python environments (isolation without containers)

Get into a `nix repl` and load the `nixpkgs`

```
$ nix repl
nix-repl> :l <nixpkgs>
````

Create a python environment with access to `numpy`

```
nix-repl> python3.withPackages (p: [p.numpy])
«derivation /nix/store/z55n2q3syn8firnk3bbi3filf7szdnrq-python3-3.11.6-env.drv»

nix-repl> :b python3.withPackages (p: [p.numpy])

This derivation produced the following outputs:
  out -> /nix/store/ag7gqm5pmjqdjf0jlwsk2y4vq0m6ldm7-python3-3.11.6-env
```

This path now has a python environment with only `numpy`

```
❯ /nix/store/ag7gqm5pmjqdjf0jlwsk2y4vq0m6ldm7-python3-3.11.6-env/bin/python
Python 3.11.6 (main, Oct  2 2023, 13:45:54) [GCC 12.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> import requests
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'requests'
```

Let's add `requests` to this environment

```
nix-repl> :b python3.withPackages (p: [p.numpy p.requests])

This derivation produced the following outputs:
  out -> /nix/store/k6xki2698nd1288gpgsa865ysvwnzlfk-python3-3.11.6-env
```

Use this python environment to have both `numpy` and `requests`

```
❯ /nix/store/k6xki2698nd1288gpgsa865ysvwnzlfk-python3-3.11.6-env/bin/python
Python 3.11.6 (main, Oct  2 2023, 13:45:54) [GCC 12.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> import requests
>>>
```

Python 3.11 with `pwntools`

```
nix-repl>:b python311.withPackages (p: [p.pwntools])

This derivation produced the following outputs:
  out -> /nix/store/z96g5xbsc748iai3kfyr7ppmzyswk6q3-python3-3.11.6-env

nix-repl> :q

❯ /nix/store/z96g5xbsc748iai3kfyr7ppmzyswk6q3-python3-3.11.6-env/bin/python
Python 3.11.6 (main, Oct  2 2023, 13:45:54) [GCC 12.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pwn
>>> from pwn import *
>>> cyclic(0x10)
b'aaaabaaacaaadaaa'
```

### Delete old unused derivations

```
nix-collect-garbage
```
