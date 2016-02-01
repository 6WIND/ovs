Open vSwitch - color output
===========================

Commits in this branch add color output to the ovs-ofctl utility. It adds an
option to the tool so as to obtain colorized output in tty, for easier reading.
Currently, only the `dump-flows` command supports colors.

Usage
-----

A new `--color` option has been added to ovs-ofctl so as to indicate whether
color markers should be used or not. It can be set to `always` (force colors),
`never` (no colors) or `auto` (use colors only if output is a tty). If provided
without any value, it is the same as `auto`. If the option is not provided at
all, colors are disabled by default.

Examples:
This first call will output colorized flows:

    ovs-ofctl dump-flows br0 --color=always

These two calls will produce colorized output on a tty, but they will not use
color markers if the output is redirected to a file or piped into another
command:

    ovs-ofctl dump-flows br0 --color=auto
    ovs-ofctl dump-flows br0 --color

These two calls will not use color markers:

    ovs-ofctl dump-flows br0 --color=never
    ovs-ofctl dump-flows br0

Below is an screenshot picturing a sample output of `ovs-ofctl dump-flows s1`
command (where `s1` is the name of a switch) under mininet. The first lines are
the “classic” non-colorized output of the tool, while the second set of flows
has been colorized through the use of the `--color` option.

![`ovs-ofctl dump-flows` output showing colors](screenshot.png)

Custom color support for output
-------------------------------

`OVS_COLORS` environment variable is parsed to extract user-defined preferences
regarding colors (this is used to set up a color theme, not to replace the
`--color` option for activating color output).

The string should be of a format similar to `LS_COLORS` or `GREP_COLORS`, with
available colors being as follows:

* ac: action field
* dr: drop keyword
* le: learn keyword
* pm: parameters receiving attributes
* pr: keyword having parenthesis
* sp: some special keywords
* vl: lone values with no parameter name

As an example, setting `OVS_COLORS` to the following string is equivalent
to using the default values:

    OVS_COLORS="ac:01;31:dr=34:le=31:pm=36:pr=35:sp=33:vl=32"

Next steps
----------

* Extend color support to other `ovs-ofctl` commands
* Extend color support to other tools (`ovs-dpctl` etc.)
* Write doc (in particular, document the `--color` option in man pages)
* Add unit tests for colorized output?
* Maybe signal it in the NEWS? (once support is complete?)
