---
layout: post
title: Parsing JSON in bash
date: 2017/03/21
---

Parse JSON in bash by piping it into Ruby:

```sh
#!/bin/sh
echo '{"foo":"bar"}' | ruby -r json -e 'puts JSON.parse(STDIN.read)["foo"]'
```

- `-r json` is equivalent to calling `require "json"` in the code
- `-e 'some_ruby_code'` will evaluate the Ruby code

### Real world

You can use this to consume a JSON API. The following will fetch your
[current rate limit status][status] from GitHub, parse the result and pass send
it to `statsd`:

```sh
#!/bin/sh
REMAINING_REQUESTS=$(curl -s https://api.github.com/rate_limit | ruby -r json -e 'puts JSON.parse(STDIN.read)["rate"]["remaining"]')
echo "github_rate_limit_remaining:${REMAINING_REQUESTS}|g" | nc -w 1 -u localhost 8125
```

[status]: https://api.github.com/rate_limit
