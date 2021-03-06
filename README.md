# QISKIT

A docker container with QISKIT and Jupyter for Raspberry Pi.

QISKIT is both a quantum simulator (so you can perform quantum computing experiments at home!) and a tool that simplifies interfacing with IBM's quantum computing services in the IBM cloud. The Quantum Experience link below will guide you through the latter if you are interested in using the Real Thing!

I just want to use the simulator for now, so I built this container to run on a Raspberry Pi 4B with 2GB RAM (with Raspberry Pi OS 10, buster).

The only prerequiste you need to install to use this container is docker. You can install docker on your Pi with this one command:

```
curl -sSL https://get.docker.com | sh
```

When done, I recommend running this command so the pi user can use docker withut sudo:

```
sudo usermod -aG docker pi
```

After executing that command, exit your shell and open a new shell. In that new shell and all subsequent shells you will be able to run `docker` commands without `sudo`. E.g., try this:

```
docker ps
```

## To build this container

NOTE: You need a little more than 2GB of RAM to build the docker container (about 3.4GB I think).  Note also that this extra memory is not needed to **run** the container, only to **build** it. A 4GB or 8GB Pi therefore won't need this step so don't bother with it. However, since I am using a 2GB machine, I increased the swap space with these commands:

```
sudo sed -i 's/CONF_SWAPSIZE=100/CONF_SWAPSIZE=1024/' /etc/dphys-swapfile
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```

Once you have that sorted out, these are the build steps:

1. Edit the Makefile to set your DockerHub ID in `DOCKERHUB_ID`

2. Edit the Makefile to set the `JUPYTER_TOKEN`. This is the token you will use to login to the Jupyter Notebook created by this container. So keep it secret. Keep it safe. :-)

3. Run `make build`. You should expect this to take a very long time. On my little Pi4B/2GB it took me more than 6.5 hours to run `make build`. Notably, the file, `lda_c_pk09.c.o` alone takes something like 30 minutes to build (and it also happens to be the first one, perhaps the only one, that causes memory to run out with just 2GB of RAM and the default 100MB of swap).

4. Optionally you can push your image to DockerHub so you don't ever have to build it again:

```
make push
```

## To run the resulting container

Once it is built, it starts up very quickly from then onward.

```
make run
```

## To use the container

Point your favorite browser at `<raspberry-pi-address>:8888/`. Enter the `JUPYTER_TOKEN` value you set in the Makefile, and you will see the familiar Jupyter Notebooks interface. I installed just one example notebook, `quantum_not_gate_qiskit.ipynb`. You can select it and run through it to verify everything is working. To create your own notebook, pull down the "New" menu at the top right, and select "Python 3".

## To learn more

This YouTube video gives a brief QISKIT primer:
    [https://www.youtube.com/watch?v=V3hXSftZuoc](https://www.youtube.com/watch?v=V3hXSftZuoc)

I took the quantum NOT gate example from this set of guided exercises:
    [https://github.com/JavaFXpert/qiskit4devs-workshop-notebooks](https://github.com/JavaFXpert/qiskit4devs-workshop-notebooks)

The IBM Quantum Experience getting started guide:
    [https://quantum-computing.ibm.com/docs/](https://quantum-computing.ibm.com/docs/)

The official QISKIT documentation:
    [https://qiskit.org/documentation/](https://qiskit.org/documentation/)


