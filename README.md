# TravisCI + LaTeX
## (using latexmk, container and deploying to Nextcloud)

---
**NOTE**

This repository is no longer maintained.

---


This is an example on how to use TravisCI.org to automatically build LaTeX documentation and upload it via WebDAV to a Nextcloud server.

The `.travis.yml` is the minimal config files used for this repository. The resulting PDF file can be found [here](https://cloud.foss-ag.de/index.php/s/U4Czc661GOZBYOG)!

I have used it for various documents in repositories under https://github.com/foss-ag

Features:
  * simple
  * fast
  * flexible

### Using `latexmk`:
If you are using TOCs, references and other things, you might need to call `latex` or `pdflatex` several times until all references are resolved. Usually a TeX-Editor does this automatically for you. It also runs everything in a non-interactive mode and only generates a log and the resulting PDF.

The tool `latexmk` automatically runs `pdflatex` as often as needed and optionally reads a configuration file called `latexmkrc`.
When run without a specific file, it will build all `*.tex` files in the current directory.

This makes it a great choice for automatic compilation of TeX files!

### Choosing the build environment:
Among other options, TravisCI allows fully featured Ubuntu 14.04 VMs with sudo permissions as the build environment. They can be enabled by adding `sudo: required` to the buildfile. But they need over a minute to boot and generally build a little slower.

A better choice is the (default) container based environment. Tho, packages need to be added via the `apt` plugin. This plugin allows to install [whitelisted](https://github.com/travis-ci/apt-package-whitelist/blob/master/ubuntu-trusty) packages.

The basic packages needed are `texlive` and `latexmk`. Installing `texlive-full` gives *all* extra LaTeX packages, but increases the build-time to over 15 minutes.

### Uploading to Nextcloud with secure credentials
The easiest way to upload to Nextcloud is via WebDAV. Since these credentials need to be known to Travis, it is a good idea to create an extra user who shares the target directory with everyone who needs access to the final files.

The deploy step uses the environment variable `$NCPW` which is hidden in the log output and thus not made public.

### Setting up TravisCI
Log into TravisCI.org with your GitHub account and enable it for the repository you want to build. Go to the repository settings, add an environment variable called `$NCPW` and make sure 'Display value in build log' is off.

Add and edit `.travis.yml.full` to your liking and commit it.

Enjoy!
