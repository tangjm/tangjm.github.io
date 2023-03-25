--- 
title: 'Bash bug'
date: '2023-03-23' 
---

I recently ran into a really gnarly bug in bash. Here is an example that captures the nub of the problem. The following script doesn't print the count when there are no files that match the pattern. See if you can figure out why.

```bash
#!/usr/bin/env bash

set -o errexit

COUNT=$(ls | grep -c pattern)
echo $COUNT
```

This behaviour is explained by the below two facts. 
1. 'grep' returns a non-zero exit status if nothing is matched.
2. The 'set -o errexit' option will terminate the script if any command returns a non-zero exit status.


To solve this problem we can use 'grep pattern | wc -l' instead of 'grep -c patten'. This works because the exit status of a pipeline is the exit status of the final command. 
Another alternative is to remove 'set -o errexit' and handle errors manually.
But if you go for the second approach, make sure you don't also have 'set -o pipefail'. The following will also not print the count if no files match the pattern.


```bash
#!/usr/bin/env bash

set -o errexit
set -o pipefail

COUNT=$(ls | grep pattern | wc -l)
echo $COUNT
```

In sum, think carefully about what your program is doing when using the 'set -o errexit' option, you might even be better off handling errors yourself but if you decide to use this option, watch out for false positives. More generally, just because something is recommended or is 'best practice' doesn't mean you should adopt the approach. Think about what you are trying to achieve and see if it fits well with your use case.
