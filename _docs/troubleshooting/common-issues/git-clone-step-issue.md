---
title: "Clone step failed: Command [git checkout $REVISION] exited with code [1]"
description: ""
group: troubleshooting
sub_group: common-issues
redirect_from:
  - /docs/troubleshooting/common-issues/git-clone-step-issue/
  - /docs/build-step-no-such-file-or-directory/
toc: true
---

## Issue

Error message:

`Clone step failed: Command [git checkout $REVISION] exited with code [1]`

## Possible cause
The GIT clone step may fail in case you have CRLF (**Windows**) end of line characters instead of LF (**Unix**).

  `Text`
{% highlight text %}
Aborting
Command [git checkout $REVISION] exited with code [1]
  [SYSTEM] Error: Failed to run git-clone step: Cloning main repository...; caused by NonZeroExitCodeError
  : Container for step title: Cloning main repository..., step type: git-clone, operation: Cloning reposit
  ory failed with exit code: 1
{% endhighlight %}

## Solution

Create a .gitattributes file in your repository (for all branches) that will enforce Git to commit files with LF.
  
## Related articles
[Troubleshooting common issues]({{site.baseurl}}/docs/troubleshooting/common-issues)