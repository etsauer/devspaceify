# DevSpace(s)-ify your exiting container images

This is a proposed approach for how to take any existing UBI-based language image, and turn it into a Dev Spaces optimized container image. Dev Spaces/Eclipse Che have introduced us to this concept that a container platform can be used, not just to host production applications, but also as a place optimized for developers to write, test and commit code. By running a devlopment workspace with an IDE inside of a container and serving it through the browser, we can extend all of the consistency. immutability and portability benefits of our application runtime environments to inner-loop development.

However, there is one challenge we have to deal with when working in this mode -- our production container images have been optimized for production. That means that they only contain the minimal tooling & libraries required to run our application and nothing more than that. In a developer workspace, we need to have a few more tools available to us to make our development experience as smooth and efficient as possible. Thus far we've solved this problem by managing completely separate images for the purpose of development and deployment. However this creates additional overhead in having to manage the build process for these two sets of images. It would be better if we could just take our existing application runtime images, and layer on top a standard set of developer tools to produce developer-optimized versions of our production images.

## Developer-optimized container images using CEkit

[CEkit](https://cekit.io) is a modular, YAML-based framework for generating Containerfile-driven images that can be built using Docker, Podman or Buildah. To acheive our developer-optimized image builds, we're going to use a 3 layered approach:

1. A base image that already contains the language, framework, or libraries we need to run our app. In this case we're selecting [language images](https://catalog.redhat.com/software/containers/search?gs&q=ubi) built on top of Red Hat's [Universal Base Images](https://catalog.redhat.com/software/base-images)
2. A _developer base_ layer that provides all the basic tools and libraries generally desired for running workspaces. Based on the [Devfile.io Developer Base Image](https://github.com/devfile/developer-images#developer-base-image).
3. A _custom tooling_ layer that contains organization or team specific tools, such as CLIs for interacting with your container platform, public cloud or other hosting environment.

With CEkit each layer that we add can be represented by 

## Building Images

To get started [download and install CEkit](https://docs.cekit.io/en/latest/handbook/installation/index.html).

To start off, let's just build our generic developer base image. This image will simply contain our _developer base_ and _custom tooling_ layers on top of a base 

Developer Base Image:

```
cekit build podman
```

