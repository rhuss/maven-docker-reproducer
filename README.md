# maven-docker-reproducer
Test-case to reproduce https://github.com/rhuss/docker-maven-plugin/issues/127

This shows that lineEnding and fileMode parameters are not honoured when building the assembly on Windows.

Steps:
 * Build the project, which should build an image `test-image`.
 * Trying to run the image will fail: 
  
  ````
  $ docker run -it test-image
  /bin/sh: ./hello.sh: Permission denied
  ````
 
 * When inspecting the image, e.g. by starting a shell in it, we can see that the permissions are 644 and not 755, and line endings are CRLF
 * This already seems to be true for the intermediate tar-file created in `target/docker/test-image/tmp/docker-build.tar`.
 
Expectation:
 * The image contains the correct fileMode and line endings.
 
