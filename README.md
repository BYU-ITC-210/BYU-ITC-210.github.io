
# Source Code to BYU IT&C 210 Supplementary Website

This [GitHub Pages](https://pages.github.com/) site contains materieals in support of [BYU Information Technology & Cybersecurity 210](https://catalog.byu.edu/engineering/school-of-technology/fundamentals-of-web-based-information-technology-1).

This repository also demonstrates a convenient way to run Jekyll locally on Windows using Docker. Doing so will help you debug your site before committing to GitHub.

## Using Docker Desktop to run Jekyll on Windows

These instructions are accurate as of December 2020. Microsoft is rapidly imporoving the Linux Subsystem for Windows so the instructions may change / improve over time.

### Install Windows Subsystem for Linux 2 (WSL 2)
Follow these instructions: [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

### Install Docker Desktop for Windows and configure to use WSL 
Follow these instructions: [https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)

### Launch a local copy of Jekyll using Docker
The correct Docker packages and configuration are contained in the `docker-compose.yml` file in the root of the site. Open a command line, change directories to the directory containing the source code and execute the following command:

```
docker-compose up
```

The first time you run this, it will download the needed docker containers which will take a few seconds. Subsequent launches will be faster.

Once the container is running, the website will be available at [http://localhost:4000](http://localhost:4000)

When you're finished testing the site return to the command line window and press CTRL-C to gracefully shut down Jekyll. Then use the following command to shut down docker.

```
docker-compose down
```
