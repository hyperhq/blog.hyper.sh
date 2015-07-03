# Writing
Create .md file in blog/source, and place image to blog/source/images

Then git push, will auto update to [https://hyper.sh/blog/](https://hyper.sh/blog/)

# Debug
If blog no change after git push, try:
``` shell
curl -X POST http://hyper.sh:8877/blog
```
to view detail error info
