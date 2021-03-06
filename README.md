# Second Voyage of HMS Beagle
[![Docker build](https://img.shields.io/badge/Docker%20build-Available-informational)](https://hub.docker.com/repository/docker/olavurmortensen/hms-beagle)

On the 27th of December 1831, the HMS Beagle embarked on its _Second Voyage_. Onboard the sloop was the young Charles Darwin, who set out on a journey that would define his career. By deploying `hms-beagle:second-voyage` we too shall embark on a journey, a journey of biology and computation.

<img src="https://raw.githubusercontent.com/olavurmortensen/hms-beagle/master/images/PSM_V57_D097_Hms_beagle_in_the_straits_of_magellan.png" width=500>

HMS Beagle is a Docker container image designed for bioinformatics. We use [Jupyter Lab](https://jupyterlab.readthedocs.io/en/stable/) as a user-interface to access the terminal, write Python notebooks, and various other things. We use the [conda](https://docs.conda.io/en/latest/miniconda.html) package manager to manage the software dependencies of all of our experiments and projects.

## Usage

### Running the container

Pull the HMS Beagle image from DockerHub.

```bash
sudo docker pull olavurmortensen/hms-beagle:second-voyage
```

Deploy a container with this image.

```bash
sudo docker run -it -p80:80 hms-beagle:second-voyage
```

### Build image from source

If you want to build the image from source, first download the source code:

```bash
git clone https://github.com/olavurmortensen/hms-beagle.git
```

Build Docker image.

```bash
cd hms-beagle
sudo docker build -t hms-beagle .
```

Run container.

```bash
sudo docker run -it -p80:80 hms-beagle
```

Proceed as in the previous section.

### Run tmux

From the container command-line, run [tmux](https://github.com/tmux/tmux/wiki) terminal multiplexer:

```bash
tmux
```

### Run Jupyter

From the container command-line, run [Jupyter](https://jupyterlab.readthedocs.io/en/stable/index.html):

```bash
jupyter lab
```

The Jupyter Lab server is now running on IP `0.0.0.0` on port `80`. Go to `0.0.0.0` in your browser and type in the token (shown in the log of the command above).

For information about how to use Jupyter, [read the docs](https://jupyterlab.readthedocs.io/en/stable/index.html).

## Description

The [Dockerfile](https://github.com/olavurmortensen/hms-beagle/blob/master/Dockerfile) defines the HMS Beagle image, and is built automatically on [Docker Hub](https://hub.docker.com/repository/docker/olavurmortensen/hms-beagle). The image extends a Ubuntu Bionic image, and installs a lot of software that you would expect in a fully-fledged Ubuntu installation (without any graphical user interface). See the [Software](#software) section for more information about the software installed.

The bash terminal is configured in the [bashrc_extra](https://github.com/olavurmortensen/hms-beagle/blob/master/bashrc_extra) file, which is appended to the image's `.bashrc`.

## Software

This section lists various software available in the image, and how they are set up.

### Conda

The [conda](https://docs.conda.io/en/latest/miniconda.html) package manager is installed via Miniconda. By default the `hms-beagle` conda environment, which is defined in the [environment.yml](https://github.com/olavurmortensen/hms-beagle/blob/master/environment.yml) file, is activated in the container.

### tmux

The [tmux](https://github.com/tmux/tmux/wiki) terminal multiplexer is installed, and configured in the [tmux.conf](https://github.com/olavurmortensen/hms-beagle/blob/master/tmux.conf) file. While usually the prefix used in tmux is `Ctrl+B`, this is changed to `Ctrl+A`, to avoid clash with browser and Jupyter shortcuts.

### Vim

The [Vim](https://www.vim.org/) text editor is installed, and is configured using the [vimrc](https://github.com/olavurmortensen/hms-beagle/blob/master/vimrc) file. The configuration adds syntax highlighting, among other things.

### Jupyter

[Jupyter Lab](https://jupyterlab.readthedocs.io/en/stable/) is installed via to the `hms-beagle` conda environment, and configured in the [jupyter_lab_config.py](https://github.com/olavurmortensen/hms-beagle/blob/master/jupyter_lab_config.py) file.

The configuration ensures that Jupyter is runnable in the container, which means the IP used is `0.0.0.0`, the port is `80`, and `--allow-root` is `True`.

The `nb_conda_kernels` package is installed to the `hms-beagle` environment, such that you can switch between Python kernels, if you have multiple conda environments with Jupyter Lab installed.

### RStudio Server

[RStudio Server](https://rstudio.com/products/rstudio/download-server/) is installed on the image. To run RStudio you need to follow the instructions below.

Run the container, and add a new user.

```
useradd [ username ]
```

Make a password for this new user:

```
passwd [ username ]
```

Then give this user rights to your working directory, for example where your data is:

```
chown -R [ username ] [ data/workdir path ]
```

Now start the RStudio Server service.

```
rstudio-server start
```

Access RStudio in the browser via `0.0.0.0:80`.
