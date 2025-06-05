---
time: 2025-06-04
---
![[Pasted image 20250604170402.png]]



```bash
ocean@ocean-VirtualBox:/opt/TQT113_linux_V2.0$ git co task-lcd-version
Switched to branch 'task-lcd-version'
ocean@ocean-VirtualBox:/opt/TQT113_linux_V2.0$ git rebase develop
Successfully rebased and updated refs/heads/task-lcd-version.
ocean@ocean-VirtualBox:/opt/TQT113_linux_V2.0$ git co task-cfg-hnd-update
Switched to branch 'task-cfg-hnd-update'
ocean@ocean-VirtualBox:/opt/TQT113_linux_V2.0$ git rebase develop
fatal: Unable to create '/opt/TQT113_linux_V2.0/.git/index.lock': File exists.

Another git process seems to be running in this repository, e.g.
an editor opened by 'git commit'. Please make sure all processes
are terminated then try again. If it still fails, a git process
may have crashed in this repository earlier:
remove the file manually to continue.
error: could not detach HEAD
ocean@ocean-VirtualBox:/opt/TQT113_linux_V2.0$ git st

```
