# Incident — Immich LXC resource starvation

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and evidence

The Immich container console became unresponsive and commands appeared to stall. The container had only 512 MB RAM for a Docker Compose stack containing the server, machine learning, PostgreSQL, and Redis.

Proxmox resource graphs showed more than 95% memory usage and more than 99% swap usage in the build record. This supported memory pressure rather than an unexplained frozen console.

## Fix and verification

CT100 was increased to **6 GiB RAM and 2 GiB swap**. The services returned healthy after the adjustment.

The checked-in [CT100 screenshot](../../evidence/proxmox/lxc-immich-resources.png) independently shows the later allocation, along with one CPU core, a 28 GB local root disk, and the media bind mount.

## Cause and limits

The allocation was insufficient for the deployed multi-service workload. The resource evidence does not establish that an OOM kill occurred, so no OOM event is invented.

The new allocation is the configuration used in this lab; it is not a guarantee of suitable performance for every library size or concurrent workload.

## Lesson

Container isolation does not remove the need for capacity planning. Use memory/swap observations and application health to size the workload, and record the allocation with the configuration evidence.
