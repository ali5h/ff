我是光年实验室高级招聘经理。
我在github上访问了你的开源项目，你的代码超赞。你最近有没有在看工作机会，我们在招软件开发工程师，拉钩和BOSS等招聘网站也发布了相关岗位，有公司和职位的详细信息。
我们公司在杭州，业务主要做流量增长，是很多大型互联网公司的流量顾问。公司弹性工作制，福利齐全，发展潜力大，良好的办公环境和学习氛围。
公司官网是http://www.gnlab.com,公司地址是杭州市西湖区古墩路紫金广场B座，若你感兴趣，欢迎与我联系，
电话是0571-88839161，手机号：18668131388，微信号：echo 'bGhsaGxoMTEyNAo='|base64 -D ,静待佳音。如有打扰，还请见谅，祝生活愉快工作顺利。

# ff [![Latest Release](https://img.shields.io/github/release/peterbourgon/ff.svg?style=flat-square)](https://github.com/peterbourgon/ff/releases/latest) [![GoDoc](https://godoc.org/github.com/peterbourgon/ff?status.svg)](https://godoc.org/github.com/peterbourgon/ff) [![Travis CI](https://travis-ci.org/peterbourgon/ff.svg?branch=master)](https://travis-ci.org/peterbourgon/ff)

ff stands for flags-first, and provides an opinionated way to populate a
[flag.FlagSet](https://golang.org/pkg/flag#FlagSet) with configuration data from
the environment. Specifically, it allows data to be parsed from commandline
args, a configuration file, and environment variables, in that priority order.

## Usage

Define a flag.FlagSet in your func main.

```go
func main() {
	fs := flag.NewFlagSet("my-program", flag.ExitOnError)
	var (
		listenAddr = fs.String("listen-addr", "localhost:8080", "listen address")
		refresh    = fs.Duration("refresh", 15*time.Second, "refresh interval")
		debug      = fs.Bool("debug", false, "log debug information")
		_          = fs.String("config", "", "config file (optional)")
	)
```

Then, call ff.Parse instead of fs.Parse.

```go
	ff.Parse(fs, os.Args[1:],
		ff.WithConfigFileFlag("config"),
		ff.WithConfigFileParser(ff.PlainParser),
		ff.WithEnvVarPrefix("MY_PROGRAM"),
	)
```

This example will parse flags from the commandline args, just like regular
package flag, with the highest priority. If a `-config` file is specified, it
will try to parse it using the PlainParser, which expects files in this format:

```
listen-addr localhost:8080
refresh 30s
debug true
```

It's simple to write your own config file parser.

```go
// ConfigFileParser interprets the config file represented by the reader
// and calls the set function for each parsed flag pair.
type ConfigFileParser func(r io.Reader, set func(name, value string) error) error
```

Finally, it will look in the environment for variables with a `MY_PROGRAM`
prefix. Flag names are capitalized, and separator characters are converted to
underscores. In this case, for example, `MY_PROGRAM_LISTEN_ADDR` would match to
`listen-addr`.
