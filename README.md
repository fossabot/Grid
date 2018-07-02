![ASCII Art Banner](images/banner.png)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmatthew-mcateer%2FGrid.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fmatthew-mcateer%2FGrid?ref=badge_shield)

[![Chat on Slack](https://img.shields.io/badge/chat-on%20slack-7A5979.svg)](https://openmined.slack.com/messages/grid)

## Modes

Grid currently has three modes.

`--tree` -- experimental

Tree is the federated learning mode.  The long term goal is to have node workers store data
locally, and train public models to make them better.

`--compute`

Compute mode lets users offer compute to train models.  Data scientists can easily publish models from 
a jupyter notebook and train them remotely.  Grid also offers easy utilities to try _n_ number of
configurations concurrently.

`--anchor`

Helps clients find workers in tree or compute mode.

# Anaconda/Python3

It is recommended you install Anaconda prior to starting the installation. The installation page for your operating system can be found [here](https://www.anaconda.com/download/).

If you just want to install python3 and pip3 this can be done through brew on MacOS or apt on Ubuntu LTS 16.04.

MacOS:

```
brew install python3
```

Ubuntu LTS 16.04:

```
apt install python3
apt install python-pip3
```

# Installing Grid

If you want to use an Anaconda environment, make sure you've created and activated it before installing anything else, e.g.:
```
conda create -n openmined python=3.6 anaconda -y
source activate openmined #(or conda activate openmined, depending on your version of conda)
```
Grid requires PyTorch.  Follow the instructions at [https://pytorch.org](https://pytorch.org) to install the correct build for your platform/environment.

Finally, navigate to the local clone of the Grid repository and run ```python setup.py install```.  If you're going to be developing Grid, you can replace that with ```python setup.py develop```.

# Launching a Worker

### Start the IPFS Peer-to-Peer Filesystem

Grid worker can now start the ipfs daemon for you, but if you want to start it manually it can be with:

```
start_ipfs
```

You can then run the worker daemon. If you want to run in `compute` mode run:

```
start_worker --compute
```

and if you want to run in `tree` mode, run:

```
start_worker --tree
```

# Launching a worker using docker

## Prerequisites

- [docker](https://docs.docker.com/) 
- [docker-compose](https://docs.docker.com/compose/)

If you have GPU and want to use them, you will need to have [nvidia-docker2](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)). Make sure you can see your gpus using

```
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi 
```

## launching the worker

The main scrip to launch the grid is **run_grid.sh**. It has several flags

```
Usage: ./run_grid.sh [-g <gpu>] [-m <grid-mode>] [-a <host_ip>] [-n <name>] [-e <email>]
```

For instance, if you want to run a **worker** in **tree** mode with an aws instance with **gpus** on it whose ip is **10.34.23.45**, then you can run

```
./run_grid.sh -g -m tree -a 10.34.23.45 -n jack -e jack@aws.com
```

If you have only a laptop without **gpus** in it, but you want to run an **anchor** because, why not

```
./run_grid.sh -m anchor -a 1.34.23.45 -n jack -e jack@laptop.com
```

# Troubleshooting

### Unable To Find Scripts

If running those commands doesn't work make sure the location of the installed script is in your PATH. This can be done by adding a the following similar line to your '~/.bash_profile' file:

```
export PATH=$PATH:/anaconda3/bin
```

`/anaconda3/` might need to be replace by the actual install location if not being used with anaconda or you installed anaconda locally for the current user.

And then running:

```
source ~/.bash_profile
```

### Trouble Running Job

If you have any troubles running an experiment such as the other peers not learning about your jobs make sure you're connected to the peer. You can check if you're connected to the peer by running:

```
ipfs pubsub peers | grep <ipfs address>
```

And then to connect to the peer if you're not connected:

```
ipfs swarm connect <ipfs_address>
```

The swarm connect IPFS address should look something like this `/p2p-circuit/ipfs/QmXbV8HZwKYkkWAAZr1aYT3nRMCUnpp68KaxP3vccirUc8`. And can be found in the output of the daemon when you start it.

## Contributing

TODO make this automatic

Before submitting a PR to this repo make sure the following packages are installed with pip:

```
pip install pycodestyle yapf pytest
```

First run `yapf`:

```
yapf --recursive --in-place .
```

Then run `pycodestyle`:

```
pycodestyle --max-line-length=130 --exclude=.ropeproject,ipfsapi,build,.eggs .
```

We still have some issues with long lines and bare excepts so those can be ignored for now. But everything else should be cleaned up.

To run the integration tests you MUST start up a local worker first (this is unfortunate see issue https://github.com/OpenMined/Grid/issues/146)

Then run the tests:

```
pytest -s
```

If everything looks good to you, submit the PR.

## License

[Apache License 2.0](https://github.com/OpenMined/Grid/blob/master/LICENSE)

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmatthew-mcateer%2FGrid.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fmatthew-mcateer%2FGrid?ref=badge_large)