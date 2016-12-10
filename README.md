# NOTICE

1. master branch is LIVE env, will update website once push
2. you can edit & preview in local on develop branch
3. you can merge develop to master once you want to publish

# Write a post

1. create a .md file with a meaningful name in blog/source folder
2. write post in the .md file with the format which **same as other posts**

# Preview on local

1. ./ink preview
2. open http://localhost:8000/

# Publish
```
git add . -A
git commit -m "update"
git push origin master
```

# How to configure the post?

[https://github.com/InkProject/ink](https://github.com/InkProject/ink)

# Troubleshoot

`Invalid format`: Try to use double quotes to wrap the string which contained some special char.
