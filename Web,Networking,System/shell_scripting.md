## Shell Scripting with example

```sh
#!/bin/bash

# e - exit immediately if a command exits with a non-zero status
# x - print commands and their arguments as they are executed
set -xe

# turn off debugging
set +x

# evaluation & interpolation
FOO=`whoami`
EVAL_FOO=`echo Port no. is $FOO not good`
echo $EVAL_FOO
CommitShort=$(git  rev-parse --short HEAD) # execute cmd and assign output to CommitShort var

# print 'Default to good if 2nd arg not provided'
Build=${2:-Default to good if 2nd arg not provided}

# reset var to an empty string
HTTP_PROXY=

# create dir recursively
mkdir a/b/c/d # will fail
mkdir -p a/b/c/d # will create a, b, c, d in the correct nested structure

# substring. arg1 - zero-based offset arg2 - length
str='123456789'
output=${str:1:5} # 23456

# func
function validate_art_creds {
  # -z <STRING> means true if string is null/empty
  if [[ -z ${ARTIFACTORY_USER} || -z ${ARTIFACTORY_PASSWORD} ]]; then
    echo "provide artifactory credentials"
    exit 1;
  fi
}
```