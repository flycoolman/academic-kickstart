---
title: 'Ubuntu 18.04 System Python 3 Upgrade'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: Change to update-alternatives broke Ubuntu system.
authors:
- admin
tags:
- Linux
- Update-alternatives
- Python 3
categories:
- Linux
- System
date: "2020-07-20T00:00:00Z"
lastmod: "2020-07-21T00:00:00Z"
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

### Symptom

After the upgrade from Python 3.6 to Python 3.8:

- Ubuntu Software Center not working
- Can't start terminal by Ctl+Alt+t
- Can't start terminal or software
- Error Message - A problem occured when checking for the updates

### Root cause

After the Python 3 upgrade, I changed the Update-alternatives configuration. Fortunately I remembered it!

### Solution

Start terminal in right click menu and change back to the original configuration of update-alternatives, as below:

![update-alternatives-config](./update-alternatives-config.png)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌
