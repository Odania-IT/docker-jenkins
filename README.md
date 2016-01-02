# docker-jenkins

This image builds a jenkins ci server. It is roughly build on the official jenkins build but uses another base image.

# Usage

```
docker run -p 8080:8080 -p 50000:50000 odaniait/docker-jenkins
```

This will store the workspace in /var/jenkins_home. All Jenkins data lives in there - including plugins and configuration.
You will probably want to make that a persistent volume (recommended):

```
docker run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home odaniait/docker-jenkins
```

This will store the jenkins data in `/your/home` on the host.
Ensure that `/your/home` is accessible by the jenkins user in container (jenkins user - uid 1000) or use `-u some_other_user` parameter with `docker run`.


You can also use a volume container:

```
docker run --name myjenkins -p 8080:8080 -p 50000:50000 -v /var/jenkins_home odaniait/docker-jenkins
```

Then myjenkins container has the volume (please do read about docker volume handling to find out more).

## Backing up data

If you bind mount in a volume - you can simply back up that directory
(which is jenkins_home) at any time.

This is highly recommended. Treat the jenkins_home directory as you would a database - in Docker you would generally put a database on a volume.

If your volume is inside a container - you can use ```docker cp $ID:/var/jenkins_home``` command to extract the data, or other options to find where the volume data is.
Note that some symlinks on some OSes may be converted to copies (this can confuse jenkins with lastStableBuild links etc)

For more info check Docker docs section on [Managing data in containers](https://docs.docker.com/userguide/dockervolumes/)
