---
layout: post
title:  "How To Create Redis Cluster"
date:   2018-06-05 12:20:32 -0500
categories: redis
---

### How To Create Redis Cluster

#### The Easy Way
1. Go to redis folder and go into folder <span style="color:Brown">/utils/create-cluster/</span>
2. Run <code>./create-cluster start</code>
3. Run  <code>./create-cluster create</code>
4. Type in <span style="color:Brown">Yes</span> for the suggest cluster configuration.
5. When you see <span style="color:Brown">[OK] All 16384 slots covered</span>, it means your cluster created successfully.
5.  Run <code>./create-cluster stop</code> to stop the cluster