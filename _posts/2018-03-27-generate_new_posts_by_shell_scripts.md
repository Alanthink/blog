---
excerpt: 
comments: true
title: generate new posts by shell scripts
categories:
  - blog build
---

## Notes

The scripts are modified from [@AamnahAkram](https://gist.github.com/aamnah/f89fca7906f66f6f6a12). For more related materials, you are suggested to visit the link.

For the scripts provided here, I improved two positions. First, you can now create a new post using `./command.sh create a new post`. You don't need to explicitly specify a space using `./command.sh create\ a\ new\ post`. Second, I removed leading blank lines caused by `echo`.

For some parts of the scripts, you need to customized by yourself.

## Scripts

```
#!/bin/bash
# About: Bash script to create new Jekyll posts
# Author: @AamnahAkram
# URL: https://gist.github.com/aamnah/f89fca7906f66f6f6a12 
# Description: This is a very basic version of the script

# VARIABLES
######################################################

# Get current working directory

# Define the post directory (where to create the file)
JEKYLL_POSTS_DIR='./_posts/'

# Post title
TITLE=""

for var in "$@"
do
    TITLE=$TITLE" $var"
done

# Trim leading spaces
TITLE="$(echo "${TITLE}" | sed -e 's/^[ \t]*//')"

# Replace spaces in title with underscores
TITLE_STRIPPED=${TITLE// /_}

# Permalink
PERMALINK=${TITLE_STRIPPED}

# Date
DATE=`date +%Y-%m-%d`

# Post Type (markdown, md, textile)
TYPE='.md'

# File name structure
FILENAME=${DATE}-${TITLE_STRIPPED}${TYPE}


# COMMANDS
#######################################################

# go to _posts directory
cd ${JEKYLL_POSTS_DIR}

# make a new post file
touch ${FILENAME}

# add YAML front matter and trim leading blank line
echo -e "
---
excerpt: ""
comments: true
title: ${TITLE}
categories:
  -
tags:
  -
---
" | sed '/./,$!d' > ${FILENAME}
```
