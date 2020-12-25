# Regular Expressions

## Lookarounds

Lookahead and lookbehind, collectively called “lookaround”, are zero-length assertions just like the start and end of line, and start and end of word anchors explained earlier in this tutorial. The difference is that lookaround actually matches characters, but then gives up the match, returning only the result: match or no match. That is why they are called “assertions”.

| **Lookaround** | **Name**            | **What it Does**                                             |
| -------------- | ------------------- | ------------------------------------------------------------ |
| (?=foo)        | Lookahead           | Asserts that what immediately follows the current position in the string is *foo* |
| (?<=foo)       | Lookbehind          | Asserts that what immediately precedes the current position in the string is *foo* |
| (?!foo)        | Negative Lookahead  | Asserts that what immediately follows the current position in the string is not *foo* |
| (?<!foo)       | Negative Lookbehind | Asserts that what immediately precedes the current position in the string is not *foo* |

### Example uses

We can match specific info in the results of terminal commands like:

```sh
# remove serial numbers in format nn:
ip link | grep -Po --regexp "(?<=[0-9]: ).*"'

# match lines till first '>' exclusively.
ip link | grep -Po --regexp ".*?(?=>)"

# list ifaces with broadcast or ppp link enabled.
ip link | grep -Po --regexp "(?<=[0-9]: ).*(?=: <BROADCAST|: <POINTOPOINT)"

# list ip addresses of active interfaces.
ip addr show | grep -Po --regexp "(?<=inet ).*(?=/[0-9]+ brd)"

# active & up iface:
ip link | grep " UP " | grep -Po --regexp "(?<=[0-9]: ).*(?=: <)"
```



## References

1. RegEx tutorial at [RexEgg](https://www.rexegg.com/)
2. [Regular-Expressions.info](https://www.regular-expressions.info/)
3. 