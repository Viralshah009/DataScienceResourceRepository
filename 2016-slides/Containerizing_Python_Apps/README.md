# Building and Deploying containerized Python Apps in the Cloud

From simple blogs and monolith Django web apps, up to sophisticated micro
service architectures, is your product ready to leverage the opportunities
brought by the new tools out there?

In this talk I show how to package Python applications as ready-to-use Docker
containers, and how to deploy and manage them in your own private cloud with
[OpenShift](https://github.com/openshift/origin).


## What's the take away?

- How to create Docker images using a Dockerfile
- Creating images with the
  [Source-to-Image](https://github.com/openshift/source-to-image) tool
- Wiring multiple components together


## Slides

http://www.slideshare.net/rhcarvalho/building-and-deploying-containerized-python-apps-in-the-cloud

## Demo

Code used during the live demo:  
https://github.com/rhcarvalho/pycon-sk-2016-demo

## Q&A

Questions asked during the talk, submitted via [sli.do](https://sli.do):

3/12/16 2:34 PM | score: 4  
**Q**: Could you compare docker based approach vs. using NixOS?  
**A**: Docker is not an operating system, it's a tool to run on top of an OS.
If we make the comparison between some form of virtualization versus bare metal,
the answer is it depends on what you're doing. Likely, you'll benefit from both:
they're complementary technologies. For containers, you're normally looking at
better utilization of resources, faster provisioning, isolation of application
components, etc.  
There are better comparisons out there in the Internet. One interesting talk
comparing containers, virtual machines and bare metal: "Containers vs.
virtualization: The new Cold War?"
[video](https://www.youtube.com/watch?v=HsqtHT8auxg),
[slides](http://videos.cdn.redhat.com/summit2015/presentations/12023_containers-versus-virtualization.pdf).

3/12/16 2:24 PM | score: 2  
**Q**: How to "hot-swap" one docker by newer one (correctly)?  
**A**: To replace a running container based on image "foo/v1" with a new
container based on image "foo/v2", there are different deployment strategies.  
In OpenShift, the supported strategies are documented
[here](https://docs.openshift.org/latest/dev_guide/deployments.html#strategies).

3/12/16 2:23 PM | score: 2  
**Q**: Should be docker image immutable? Or is it secure to upgrade it by
some inner upgrade? Or should it be as the last layer in regular
test/deploy/deliver step?  
**A**: An image itself is immutable. Running something like `yum update` creates
a new layer, and that's technically a new image. You can, however, tag it with
the same tag as your previous image, giving a feel of mutation.  
In most cases, it's more efficient to rebuild images when a parent image change,
for example to include security updates, rather than running some update command
on every derived image.  
OpenShift makes it easy to implement that flow through [image change
triggers](https://docs.openshift.org/latest/dev_guide/builds.html#image-change-triggers).

3/12/16 2:20 PM | score: 3  
**Q**: When using docker for development, is there any way to start python
debugger (pdb, ipython, bpython) inside docker container and debug
from within docker?  
**A**: Yes, use [`docker run`](https://docs.docker.com/engine/reference/run/)
and [`docker exec`](https://docs.docker.com/engine/reference/commandline/exec/)
to run your Python programs interactively.  
For example, to open an IPython session in a new ephemeral (because of `--rm`)
container:
```console
$ docker run --rm -it python:3.5 bash -c 'pip install ipython && ipython'
...
Python 3.5.1 (default, Mar  9 2016, 03:30:07)
Type "copyright", "credits" or "license" for more information.

IPython 4.1.2 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]:
```
To get your code mounted from the host to the container, use the `--volume` flag
when starting the container with `docker run`. Consult the documentation for
details on how to use it.  
With OpenShift, the [Web
Console](https://docs.openshift.org/latest/architecture/infrastructure_components/web_console.html)
makes it really easy to run commands in a live container directly from your Web
browser.

3/12/16 2:19 PM | score: 3  
**Q**: Is s2i creating the Dockerfile for me?  
**A**: It doesn't create Dockerfiles, instead it goes further and creates Docker
images.

3/12/16 2:10 PM | score: 13  
**Q**: does it make sense to use virtualenv inside docker?  
**A**: Yes, it does. A good explanation about this topic can be found
[here](http://blog.dscpl.com.au/2016/01/python-virtual-environments-and-docker.html).

3/12/16 2:09 PM | score: 6  
**Q**: How would you run "django-admin.py upgrade" in your container? (or
just run some other manually ran management commands)  
**A**: If you're using just Docker, `docker exec`. If using OpenShift to manage
your containers, then there's a similar command `oc exec`. There's also `oc rsh`
to start a remote shell in the container, so you can run arbitrary commands
interactively. There's also a remote shell in the Web Console, so you can run
commands in your container straight from your Web browser.

3/12/16 2:07 PM | score: 8  
**Q**: What about security? If you are using image from dockerhub you can't
be sure about it. it's good for production usage?  
**A**: Security requirements vary a lot. The [OpenShift
documentation](https://docs.openshift.org/latest/welcome/index.html) has more
information about [container and platform
security](https://docs.openshift.org/latest/install_config/install/prerequisites.html#security-warning).

3/12/16 2:07 PM | score: 7  
**Q**: Are there any practical differences between Docker and the other
containerization technologies (OpenVZ, ..), or is it just about the
'cool factor'?  
**A**: Docker is not the first container technology, but it's the current
industry standard for container image format and distribution.

3/12/16 2:06 PM | score: 7  
**Q**: can you tell us what is special about OpenShift compared to other similar
tools?  
**A**: OpenShift is optimized for continuous application development and
multi-tenant deployment. It adds developer and operations-centric tools on
top of [Kubernetes](https://k8s.io) to enable rapid application development,
easy deployment and scaling, and long-term lifecycle maintenance for small and
large teams. More information about features and architecture can be found in
[the project's
README](https://github.com/openshift/origin#openshift-application-platform).

3/12/16 2:05 PM | score: 8  
**Q**: You told "python:3.5" is "official Python image'. Was it packaged by
Python people || Docker poeple || some random guy? Who will take care
of Python upgrade?  
**A**: It is packaged by Docker, Inc. It comes from this
[repository](https://github.com/docker-library/python).

3/12/16 1:56 PM | score: 7  
**Q**: What is biggest business advantage of using docker ?  
**A**: There are
[several](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/7.0_Release_Notes/sect-Red_Hat_Enterprise_Linux-7.0_Release_Notes-Linux_Containers_with_Docker_Format-Advantages_of_Using_Docker.html).
