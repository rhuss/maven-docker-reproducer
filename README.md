# maven-docker-reproducer
Test-case to reproduce https://github.com/rhuss/docker-maven-plugin/issues/127

This shows that lineEnding parameter is not honoured when building the assembly on Windows.

Steps:
 * Build the project, which should build an image `test-image`.
 * Trying to run the image will fail:

  ````
  $ docker run -it test-image
  ': No such file or directory

  alexander.krauss@QAMUC-NB-0582 /l/oss/maven-docker-reproducer (master)
  $ docker run -it test-image sh
  /maven # hexdump hello.sh
  0000000 2123 622f 6e69 652f 766e 7320 0d68 0d0a
  0000010 650a 6863 206f 4822 6c65 6f6c 7720 726f
  0000020 646c 222e 0a0d
  0000026
  $
  ````

 * When inspecting the image, e.g. by starting a shell in it, we can see that the line endings are CRLF
 * This already seems to be true for the intermediate tar-file created in `target/docker/test-image/tmp/docker-build.tar`.

Expectation:
 * The image contains the correct line endings.

