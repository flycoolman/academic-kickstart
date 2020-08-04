---
title: 'Build A Free Website In Minutes - Part 5'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: Learn how to create the environment on another machine.
authors:
- admin
tags:
- Hugo
- Web Development
- Static Website
- Github Pages
- Academic
categories:
- Basic
- Web Development
date: "2020-08-04T00:00:00Z"
lastmod: "2020-08-04T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  caption: ''
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
The website has been up and running for a while. Today I want to update a document on another machine. The steps below describe how to set up the environment on the machine.

## Steps

- Install Hugo Extended version
- Gitclone the repository
- Delete the 'public' folder
- Update the submodule
- Checkout the branch 'master'
- Work on the document
- Add, Commit and Push as before

### Step 1: Install Hugo Extended version

    wget https://github.com/gohugoio/hugo/releases/download/v0.74.3/hugo_extended_0.74.3_Linux-64bit.deb

    sudo apt install ./hugo_extended_0.74.3_Linux-64bit.deb 

### Step 2: Gitclone the repository

    git clone https://github.com/flycoolman/academic-kickstart.git

### Step 3: Delete the 'public' folder

    cd academic-kickstart/
    rm -rf public

### Step 4: Update the submodule

    git submodule update --init --recursive

### Step 5: Checkout the branch 'master'

    cd public
    git checkout master

### Step 6: Work on the document

    blahblahblah...

### Step 7: Add, Commit and Push as before

    cd public
    git add .
    git commit -m "blahblah..."
    git push

    cd ..
    git add .
    git commit -m "blahblah..."
    git push

ALL SET!

<br>

## Errors and Tricks

### Error 1: Unable to locate template for shortcode

>hongwei@HP840G1:~/Documents/academic-kickstart$ hugo server  
>Building sites … ERROR 2020/08/04 07:51:50 Unable to locate template for shortcode "fragment" in page "slides/example/index.md"  
>ERROR 2020/08/04 07:51:50 Unable to locate template for shortcode "alert" in page "talk/example/index.md"  
>ERROR 2020/08/04 07:51:50 Unable to locate template for shortcode "alert" in page "publication/preprint/index.md"  
>ERROR 2020/08/04 07:51:50 Unable to locate template for shortcode "alert" in page "post/build-a-free-website-in-minutes-part-1/index.md"  

**Solution**  
Install Hugo Extended version.

### Error 2: failed to extract shortcode

>hongwei@HP840G1:~/Documents/academic-kickstart$ hugo server  
>Built in 11 ms  
>Error: Error building site: "/home/hongwei/Documents/academic-kickstart/content/home/demo.md:61:1": failed to extract shortcode: template for shortcode "alert" not found  

**Solution**  
If the Hugo Extended version has been installed, update the git submodule, see **Step 4** above.

### Error 3: 'public' already exists

>hongwei@HP840G1:~/Documents/academic-kickstart$ git submodule update --init --recursive  
>Submodule 'public' (https://github.com/flycoolman/flycoolman.github.io.git) registered for path 'public'  
>Submodule 'themes/academic' (https://github.com/gcushen/hugo-academic.git) registered for path 'themes/academic'  
>fatal: destination path '/home/hongwei/Documents/academic-kickstart/public' already exists and is not an empty directory.  
>fatal: clone of 'https://github.com/flycoolman/flycoolman.github.io.git' into submodule path '/home/hongwei/Documents/academic-kickstart/public' failed  

**Solution**  
Delete the 'public' folder, and update the git submodule again.

### Error 4: forgot to check out before working on document

>hongwei@HP840G1:~/Documents/academic-kickstart/public$ git status  
>HEAD detached from f82be3e  
>nothing to commit, working tree clean  

>hongwei@HP840G1:~/Documents/academic-kickstart/public$ git push  
>fatal: You are not currently on a branch.  
>To push the history leading to the current (detached HEAD)  
>state now, use  
>  
>    git push origin HEAD:<name-of-remote-branch>  

>hongwei@HP840G1:~/Documents/academic-kickstart/public$ git branch  
>* (HEAD detached from f82be3e)  
>  master  

>hongwei@HP840G1:~/Documents/academic-kickstart/public$ git checkout master  
>Warning: you are leaving 1 commit behind, not connected to  
>any of your branches:  
>  
>  1090675 revise Upgrade to VirtualBox 6.1 and Vagrant 2.9.9 on Ubuntu 18.04  
>  
>If you want to keep it by creating a new branch, this may be a good time  
>to do so with:  
>  
> git branch <new-branch-name> 1090675  
>  
>Switched to branch 'master'  
>Your branch is up to date with 'origin/master'.  

**Solution**  

    git branch temp 1090675
    git push --set-upstream origin temp
    git checkout temp
    git push

Merge the branch **temp** to **master**

    git checkout master 
    git pull

    git branch --merged
    git branch -vv
    git branch -d temp

<br>

#### Did you find this page helpful? Consider sharing it 🙌
