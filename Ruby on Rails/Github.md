Github Tips

remove something from inside a git directory but not remove it from local project folder

```
git rm -r --cached myFolder
```


Switching push/pulls between Github and Bitbucket

You can use multiple remote repositories with git. Check this: [http://gitref.org/remotes/](http://gitref.org/remotes/)

But you'll have to push separately into 2 of your remotes I believe.

For example, if your project currently points to github, you can rename your current remote repository to `github`:

```
$ git remote rename origin github

```

You can then add another remote repository, say `bitbucket`:

```
$ git remote add bitbucket git@bitbucket.org:your_user/your_repo.git

```

Now in order to push changes to corresponding branch on github or bitbucket you can do this:

```
$ git push github HEAD
$ git push bitbucket HEAD

```

Same rule applies to pulling: you need to specify which remote you want to pull from:

```
$ git pull github your_branch
$ git pull bitbucket your_branch
```

##### Github safe merge 

```git
git checkout master
git pull origin master
git merge test_branch
git push origin master
```







