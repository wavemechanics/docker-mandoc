This is a wrapper to run mandoc from a docker container instead of installing the
mandoc package.

This is mainly to allow a project to have man pages without requiring developers to
install `mandoc` locally if they already have Docker installed.

# How to use

1. Clone this repo

2. Build the image:

	docker build -t local/mandoc:latest .

3. Create the wrapper

   Create a file named `mandoc.sh` somewhere in your PATH looking like this:

       #! /bin/sh
       exec docker run -i -v `pwd`:/source --rm local/mandoc /usr/bin/mandoc "$@"

   Make sure it executable.

4. Use `mandoc.sh` instead of `mandoc` to format man pages:

	mandoc.sh file.mandoc

If docker engine is on another host, you won't be able to use bind mounts, but you can
redirect your mandoc file to `mandoc` on stdin.
Create `mandoc.sh` like this:

	#! /bin/sh
	exec docker run -i --rm local/mandoc /usr/bin/mandoc 

And then invoke like this:

	mandoc.sh < file.mandoc
