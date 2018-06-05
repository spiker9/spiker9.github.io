---
layout: post
title:  "How To Use Git"
date:   2018-06-05 10:20:32 -0500
categories: git
---

### How to use git ??? (Continious Updating)
1. **Create new Branch**
	
    <code>git checkout -b <new branch name> [<existing branch name> default to master]</code>

	<code>Git commit -m ""</code>
	
    <code>Git push --set-upstream <up-stream branch> <new branch name></code>
2. **Clone specific branch from remote repository**
	
    <code>git clone -b my-branch repository_url</code>
	
    With Git 1.7.10 and later, add --single-branch to prevent fetching of all branches. Example, with OpenCV 2.4 branch:
	
    <code>git clone -b opencv-2.4 --single-branch <repository URL></code>

3. **Restore a deleted local repository file**

    - Get revision number
    
    <code>git rev-list -n 1 HEAD -- <file_path></code>
    
    - Restore to the above revision number

    <code>git checkout <revision number> -- <file_path></code>