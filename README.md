# Howto document OpenWebRX+ source with Doxygen

## Introduction

Doxygen is a way to document source code (Python, C++, JavaScript) and render it as local webpages.

My approach is to do this on a Windows PC, so I can use the latest version of Doxygen for other projects as well.

Note: My OpenWebRX+ instance is running on a Raspberry Pi 5.

## Resources:

* Doxygen: https://www.doxygen.nl/
* OpenWebRX+ Source: https://github.com/luarvique/openwebrx


## Preparations

  * Obviously, you have to install OpenWebRX+ on your Linux machine (Raspberry Pi 5) first, including all the source code.
  * On the Linux machine:
    * use Samba to share the folder containing the OpenWebRX+ source code, and on the windows PC, map drive O: to \\<linuxhost>\openwebrx. 
    * Note: I am sorry, but I will not go into the details of how to set this up. A good starting point is this [How to Setup a Raspberry Pi Samba Server.](https://pimylifeup.com/raspberry-pi-samba/)
  * On the PC:
    * Install the latest version of Doxygen from https://www.doxygen.nl/download.html
    * Install the latest version of the "dot" tool from https://www.graphviz.org/ This will create caller and called graphs.
    * Create a folder with a sensible name, e.g. `openwebrx-source-documentation`
    * In this folder create the configuration file 'DoxyFile' with the command `doxygen -g`.
    * Edit `DoxyFile` to change variables like `INPUT` and `OUTPUT_DIR` as shown in the example below.
    * Open a CMD window and change the directory to the folder `openwebrx-source-documentation`
    * Run the command `doxygen`. It will use `DoxyFile`in that current folder as the configuration file.
    * Building the documentation may take a 3..5 minutes and will create around 342MB of output in 24000 files.
    * The output is in the folder `.openwebrx-source-documentation\build\html\`, so use `openwebrx-source-documentation\build\html\index.html` to show the generated documentation.

  ## Example of the relevant part of the /etc/samba/smb.conf

  On the Linux machine, add the following to `/etc/samba/smb.conf` to share the OpenWebRX+ source code:

    [openwebrx]
      path = /home/<your_username>/openwebrx
      browsable = yes
      read only = no
      public = yes
      writable = yes
      only guest = no
      create mask = 0777
      directory mask = 0777

## Example of the relevant parts to change in Doxyfile

The most important bits to change in the configuration file are:
	
    PROJECT_NAME           = OpenWebRX+
    PROJECT_NUMBER         = 1.2.110	(or higher, following the current version of openwebrx+. This is cosmetic only)
    OUTPUT_DIRECTORY       = build
    FULL_PATH_NAMES        = NO
    OPTIMIZE_OUTPUT_JAVA   = YES
    EXTENSION_MAPPING      = js=javascript
    EXTRACT_ALL            = YES
    INPUT                  = O:        (The mapped network drive)
    FILE_PATTERNS          = ...
                             *.qsf \
                             *.ice \
                             *.js    (Add *.js to the list of FILE_PATTERNS)
    RECURSIVE              = YES
    EXCLUDE                = O:\attic \
                             O:\owrx-build \
                             O:\test
    SOURCE_BROWSER         = YES
    INLINE_SOURCES         = YES
    REFERENCED_BY_RELATION = YES
    REFERENCES_RELATION    = YES
    GENERATE_LATEX         = NO
    HAVE_DOT               = YES
    CALL_GRAPH             = YES
    CALLER_GRAPH           = YES
    DIR_GRAPH_MAX_DEPTH    = 2
