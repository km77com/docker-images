# Docker images
Dockerfile configurations for images published at [https://hub.docker.com/u/km77/](https://hub.docker.com/u/km77/)

Every folder holds one image's `Dockerfile`; images are published to
[km77/docker-images](https://hub.docker.com/r/km77/docker-images) by the Jenkins job `miscellanea/docker-images`.

To publish a new image version:

1. Create a **new** git tag named `<folder>-v<N>`, e.g. `php-7.4-apache-v13`, and push it:
   ```bash
   git tag php-7.4-apache-v13
   git push origin php-7.4-apache-v13
   ```
2. In Jenkins, run `miscellanea/docker-images` with **Build with Parameters**, setting `TAG` to the tag you pushed.

The job builds `<folder>/Dockerfile` at that tag and pushes it as `km77/docker-images:<tag>`
(e.g. `km77/docker-images:php-7.4-apache-v13`).

To add a new image, create a new folder with its `Dockerfile` and tag it as above.
