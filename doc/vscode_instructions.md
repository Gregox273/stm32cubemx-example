# VSCode Specific Instructions

## Dev Container

VSCode allows for remote development within a container - developing within a container allows for a well-defined development environment that is separated from the host machine.

## Debugging
The project configuration in this repository makes use of the [Cortex-Debug](https://github.com/Marus/cortex-debug) extension. 

The configuration in launch.json assumes a GDB server (e.g. OpenOCD) is running on the host machine, outside the development container. This is necessary as Windows hosts do not allow COM ports to be passed through to a container.