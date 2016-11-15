+++
date = "2016-11-15T11:30:00+01:00"
title = "Github images in the wiki"
+++

# Directly embedding images into a Github wiki

Although there's a reasonable amount of posts saying that you have to upload a
image to your repository (the code), and then link with the "raw" approach, my
colleague and I found a more elegant solution:

> Upload the image directly into the Wiki repository

Indeed the wiki is just another Git repo ! Here is the [github
page](https://help.github.com/articles/adding-and-editing-wiki-pages-locally/).

So basically you need first clone the wiki repo:
`git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.wiki.git`

Then you add your image:
```
cp ~/myimage.png YOUR_REPOSITORY.wiki/
cd YOUR_REPOSITORY.wiki
git add myimage.png
git commit -m "added picture for the wiki"
git push
```

Your image is now available at
`https://raw.githubusercontent.com/wiki/USERNAME/REPOSITORY/myimage.png`

so you only need to link it in your Wiki using the regular link
`[text]("https://raw.githubusercontent.com/wiki/USERNAME/REPOSITORY/myimage.png")`



