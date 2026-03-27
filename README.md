### Remarks (added 2025)

_This project is part of my Masters Thesis and presented at the BSD conferences in Asia, Europe and Canada in 2012 and 2013._

_Paper: [SCTP in Go](https://2013.asiabsdcon.org/papers/abc2013-P7A-paper.pdf)_

_Presentation: [Implementation of SCTP in Go (FreeBSD) - Olivier Van Acker, EuroBSDcon 2012](https://www.youtube.com/watch?v=0bsxjosoSoc)_

_The SCTP implementation is part of the Go runtime / standard library and implemented in exactly the same manner TCP and UDP are implemented;_
_directly linked to the relevant system calls in the kernel._

_This implementation is a reimplementation of the original version which was implemented when Go 1.0 was released._
_It only works on old versions of FreeBSD and MacOS (11 and El Captain)._ 


# SCTP in Go

This branch has [SCTP](https://en.wikipedia.org/wiki/Stream_Control_Transmission_Protocol) functionality added to the network library.
I will try and keep this branch up to date with the master branch of the [official Go repository](https://github.com/golang/go)

## Go wild!
Needless to so say, SCTP in Go is experimental and should be used with caution.  

## Supported platforms 
For now this will only work on [FreeBSD 11](https://www.freebsd.org/) and [OSX EL Captain](http://www.apple.com/uk/macos/sierra). 

FreeBSD comes with native SCTP support. On Mac OSX follow the instructions in the 
[official SCTP repository](https://github.com/sctplab/SCTP_NKE_ElCapitan) to install the driver.

MacOS Sierra is not supported since there are no SCTP drivers available yet.

Example server:

```golang
package main
import (
	"log"
	"os"
	"net"
)

func main() {

	saddr := "127.0.0.1:4242"
	addr, err := net.ResolveSCTPAddr("sctp", saddr)
	if err != nil {
		println(err)
		os.Exit(-1)
	}

	conn, err := net.DialSCTP("sctp", nil, addr)
	if err != nil {
		println("Error listening " + err.Error())
		os.Exit(-1)
	}

	defer conn.Close()

	var message = "hello"
	bmessage := []byte(message)

	_, err = conn.WriteTo(bmessage, addr)
	if err != nil {
		log.Printf("WriteTo error: %v", err)
	}

}
```

## Build instructions for SCTP in Go
These commands are based on the instructions [here](https://golang.org/doc/install/source).

	$ git clone https://github.com/3vilM33pl3/go-sctp-thesis
        $ export GOROOT=/usr/local/go 
	$ cd go-sctp-thesis
	$ cd src
	$ ./all.bash

To run the go command replace symbolic link /usr/local/bin/go and point it to go-sctp-thesis/bin/go:

        $ rm /usr/local/bin/go
        $ ln -s /pathtorepo/go-sctp-thesis/bin/go /usr/local/bin/go

## Test SCTP in Go
The [SCTP examples repository](https://github.com/cyberroadie/sctp-examples) contains working examples 
and instructions to test SCTP. It has TCP examples to compare with.

## Have fun!
Any questions drop me an email 'ovanac01 at mail.bbk.ac.uk' or tweet #cyberroadie

Olivier Van Acker

### Below this line is the official Go README documentation 


# The Go Programming Language

Go is an open source programming language that makes it easy to build simple,
reliable, and efficient software.

![Gopher image](doc/gopher/fiveyears.jpg)

For documentation about how to install and use Go,
visit https://golang.org/ or load doc/install-source.html
in your web browser.

Our canonical Git repository is located at https://go.googlesource.com/go.
There is a mirror of the repository at https://github.com/golang/go.

Go is the work of hundreds of contributors. We appreciate your help!

To contribute, please read the contribution guidelines:
	https://golang.org/doc/contribute.html

##### Note that we do not accept pull requests and that we use the issue tracker for bug reports and proposals only. Please ask questions on https://forum.golangbridge.org or https://groups.google.com/forum/#!forum/golang-nuts.

Unless otherwise noted, the Go source files are distributed
under the BSD-style license found in the LICENSE file.

--

## Binary Distribution Notes

If you have just untarred a binary Go distribution, you need to set
the environment variable $GOROOT to the full path of the go
directory (the one containing this file).  You can omit the
variable if you unpack it into /usr/local/go, or if you rebuild
from sources by running all.bash (see doc/install-source.html).
You should also add the Go binary directory $GOROOT/bin
to your shell's path.

For example, if you extracted the tar file into $HOME/go, you might
put the following in your .profile:

	export GOROOT=$HOME/go
	export PATH=$PATH:$GOROOT/bin

See https://golang.org/doc/install or doc/install.html for more details.
