# Dockerfiles for ArchiveBot components

This repository contains Dockerfiles for running [Archive Team ArchiveBot](https://github.com/ArchiveTeam/ArchiveBot) components.  These Dockerfiles were developed with the goal of running ArchiveBot components on [CoreOS](https://coreos.com/), but should be applicable to any Docker deployment.

At present, this repository contains a Dockerfile for ArchiveBot's downloader pipeline.

# The `pipeline` Dockerfile

The `pipeline` Dockerfile runs an instance of ArchiveBot's downloader program.  The downloader is configured to run one job at at time.  (If you want to handle multiple jobs, you can adjust the Dockerfile or just run multiple `pipeline` containers.)  

It assumes:

1. The existence of a Redis connection available via `redis://redis:6379/0`
2. An external volume for job data mounted in the container at `/home/archivebot/ArchiveBot/pipeline/data`
3. An external volume for WARCs to be uploaded mounted in the container at `/home/archivebot/warcs4fos`
4. The pipeline name is supplied as the `PIPELINE_NAME` environment variable

The recommended way to do this is:

1. Set up a container named `redis` running an SSH tunnel to an ArchiveBot control node's Redis instance 
2. Use the `-v` switch of `docker run` to mount the volumes
3. Use the `-e` switch of `docker run` to set `PIPELINE_NAME`

Example:

```
# this assumes the "redis" container already exists
docker run -e PIPELINE_NAME=some-pipeline-name \
  -v /mnt/data/pipeline:/home/archivebot/ArchiveBot/pipeline/data \
  -v /mnt/data/warcs4fos:/home/archivebot/warcs4fos \
  --link redis
  -i [IMAGE ID]
```

If you were planning on starting and stopping many pipelines, you could also do this by setting up a Docker network.

[TODO]

# The `uploader` Dockerfile

[TODO]
