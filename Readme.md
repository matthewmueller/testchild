# Testchild

[![Go Reference](https://pkg.go.dev/badge/github.com/matthewmueller/testchild.svg)](https://pkg.go.dev/github.com/matthewmueller/testchild)

Easily test subprocesses in `go test`.

Inspired by the subprocess testing section in [GopherCon 2017: Mitchell Hashimoto - Advanced Testing with Go](https://youtu.be/8hQG7QlcLBk?t=2031).

## Install

```sh
go get github.com/matthewmueller/testchild
```

## Example

```go
package my_test

import (
	"io"
	"os"
	"os/exec"
	"strings"
	"testing"

	"github.com/matryer/is"
	"github.com/matthewmueller/testchild"
)

func TestRun(t *testing.T) {
	is := is.New(t)
	parent := func(t testing.TB, cmd *exec.Cmd) {
		cmd.Stdin = strings.NewReader("hello")
		is.NoErr(cmd.Start())
		is.NoErr(cmd.Wait())
	}
	child := func(t testing.TB) {
		is := is.New(t)
		buf, err := io.ReadAll(os.Stdin)
		is.NoErr(err)
		is.Equal(string(buf), "hello")
	}
	testchild.Run(t, parent, child)
}
```

## Development

First, clone the repo:

```sh
git clone https://github.com/matthewmueller/testchild
cd testchild
```

Next, install dependencies:

```sh
go mod tidy
```

Finally, try running the tests:

```sh
make test
```

## License

MIT
