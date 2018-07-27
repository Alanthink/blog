---
excerpt: ""
comments: true
title: Generate new posts by shell scripts
categories:
  - blog build
---

The script is modified from [@AamnahAkram](https://gist.github.com/aamnah/f89fca7906f66f6f6a12). For more related materials, you are suggested to visit the link.

For the script provided here, I made three small modifications.

*  You can now create a new post using `./command.sh [your post name]` while not explicitly specifying spaces using `./command.sh [your\ post\ name]`
*  Spaces in title are replaced with hyphen
*  Leading blank lines caused by `echo` are removed

For further requirements, you may need to customize it by yourself.

## Script

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
TITLE=''

for var in "$@"
do
    TITLE=$TITLE" $var"
done

# Replace spaces in title with hyphen
TITLE_STRIPPED=${TITLE// /-}

# Permalink
PERMALINK=${TITLE_STRIPPED}

# Date
DATE=`date +%Y-%m-%d`

# Post Type (markdown, md, textile)
TYPE='.md'

# File name structure
FILENAME=${DATE}${TITLE_STRIPPED}${TYPE}


# COMMANDS
#######################################################

# go to _posts directory
cd ${JEKYLL_POSTS_DIR}

# make a new post file
touch ${FILENAME}

# add YAML front matter and trim leading blank line
echo -e "
---
excerpt: \"\"
comments: true
title: ${TITLE}
categories:
  -
tags:
  -
---
" | sed '/./,$!d' > ${FILENAME}
```
