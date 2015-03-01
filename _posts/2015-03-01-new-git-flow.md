---
layout:     post
title:      Git Flow for Selective Feature Deployment
date:       2015-03-01 02:30:00
summary:    Here I propose a slightly modified git flow than what most startups use. Read more if you aren't satisfied with your existing git branching model.
categories: git
---

#### GitHub Flow
If you are just starting on a project with multiple developers in the team, [GitHub flow](http://scottchacon.com/2011/08/31/github-flow.html) could be sufficient for your case. It basically works on the master branch, feature branches are created from master and then merged back into it after testing. The problem with this flow is every developer will have to test his branch separately, if you have a QA or staging environment for testing, each feature branch will have to be deployed on it separately, resulting in more waiting time for testing.


#### Git Flow
If your development team is growing and you are looking for more organized way to manage your git branches and deployment workflow, you must read [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/). Its being used and recommended by many developers and startups. 
Basically git-flow relies on a `develop` branch where all the feature branches are merged and a release candidate is taken out of it. This release candidate branch then gets tested, fixed and waits for its turn to go live(i.e. to be merged into `master`). Hotfixes are directly merged to master. Every merge/commit to master is tagged. This workflow will work for most of the cases, but the problem is that `develop` branch contains commits from all the feature branches currently being tested in staging environment, this means if at the end of your development cycle, one of the feature is not ready to go live, you will have to wait for the production deploy or take the feature branch out of `develop`, both the options aren't always feasible.

So here is a slightly modified Git Flow that we use at Appthority:

#### Proposed Git Flow

![Appthority Git Flow](/images/git-flow.jpg)

* Create feature branches from `release_candidate`
* Merge your feature branch into `staging`, no need for pull request here
* __Do not merge staging back into your feature branch ever__
* Resolve merge conflicts between feature and staging branch __inside staging branch__
* Deploy and test staging branch in staging/QA environment
* When the feature is QA verified, create a Pull Request for your feature branch to `release_candidate` and merge.
* All the code that goes into release_candidate must be ready for production deploy
* Pull Requests for hotfixes go directly into `master`, create hotfix branches from master.
* Now in this flow, only the QA verified feature branches go into `release-candidate`, this means if there is a feature that didn't get verified during the sprint can still continue to be tested in staging environment but won't be deployed to production.
* Another benefit is that developers won't get blocked on code review as the code review process can be done when merging the feature branch into `release-candidate`, which happens at the end of the testing.
* Your staging branch might get a bit messed up with commits, you should delete and recreate(or force push) staging branch at the start of each sprint from `release-candidate`

Let me know if you use this flow in your development, and how that works out :)