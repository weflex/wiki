In this page, we will list some known issues and its solution(s).

### bundled script can't be respond completely

This issue is caused by the server disk is used up, and the Node.js static service is going to load the file into swap/disk.

Issue: https://github.com/weflex/wechat/issues/116.
Status: solved and this is mostly a deployment and operating issue.

To clean up obsolete docker images and free up disk space, do:

```bash
#!/bin/bash
# remove intermediate images
docker rmi `docker images | grep <none>`

# remove dead images
docker rmi `docker images -aq`
```

Result after solution: disk usages shrinked from 20GB (out of 20GB) to 7GB (70% reduced).