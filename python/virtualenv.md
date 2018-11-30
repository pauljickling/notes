## Virtual Environment

The best practice when working Python projects is to create a virtual environment so that configuration options are isolated from other projects that are being worked on.

### Setup

From the terminal run:

```
virtualenv env
source env/bin/activate
```

Once that is setup you can go ahead and install dependencies via pip.

One useful flag when creating your directory's virtual environment is to specify the version of Python to use. This is how that is implemented:

`virtualenv --python=python3.6 env`

### Exiting Virtual Environment

The virtual environment can be exited via the `deactivate` command.
