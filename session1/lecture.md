Mesosphere K8S Bootcamp
=======

Session 1
----

Container Basics



Learning Objectives
====

* Understand container basics (cgroups, namespaces, chroot) 
* Understand Docker (images, registry, etc.)
* Mostly slides, some instructor-executed examples



Section 1: Container Basics
====

* What is a container anyway?
* cgroups
* namespaces
* chroot



Containers
====

Kind of like a mini close to the metal virtual machine for Linux.

Or, taking groups of processes and separating them for deployment on a single machine.



Why?
====

Partially for deployment security and simplification.

Partially for multi-tenancy hosting situations.

With containers you deploy groups of processes in one move:

    RoR Web App + MySQL Database + Redis 



How It Works 1: chroot
====

The first level of understanding is the chroot API.  Python example:

    os.chroot(root)
    os.chdir("/tmp")
    os.setgroups([])
    os.setgid(running_gid)
    os.setuid(running_uid)
    os.umask(077)


chroot
====

This has been standard good process hygene for years.

Change root to a safe directory, drop privilege, force secure files.



How it Works 3: cgroups
====

What about the network? Disk isolation? Firewalls? Multiple processes?

Enter cgroups, a kind of super chroot.



cgroups
====

You can do the following with cgroups today:

* Resource limitation limiting memory, file, cache.
* Prioritization of CPU, disk I/O.
* Accounting to measure resource usage.
* Control to freeze a group, restart it, checkpointing.



How it Works 2: namespaces
====

While cgroups control how much something gets, namespaces provide more isolation:

* The PID namespace for process IDs.
* Network namespace for network interfaces.
* "UTS" namespace to fake change the hostname.
* Mount namespace for fake filesystem layouts.
* IPC namespace to separate the System V IPC mechanisms.
* User namespace separate user IDs.



kernfs
====

Related to this is the work by Tejun Heo on *kernfs*.

This restructured how cgroups works so the system is simpler.

Currently available in 3.16?



cgroups Documentation
====

The official docs seem to be here:

https://www.kernel.org/doc/Documentation/cgroups/cgroups.txt

These may be old given the current state of cgroups.



Putting It All Together
====

This is all encapsulated into a library called libcontainer:

https://github.com/opencontainers/runc/tree/master/libcontainer

It's moved a bit, so be conscious of which one you're using.



Using cgroups Manually
====
Only works in CentOS 6.5.

mount -t tmpfs cgroup_root /cgroup
cd /cgroup
mkdir cpuset
mount -t cgroup -ocpuset cpuset /cgroup/cpuset



Using cgroups Manually
====

mkdir Charlie
cd Charlie
/bin/echo 2-3 > cpuset.cpus
/bin/echo 1 > cpuset.mems
/bin/echo $$ > tasks
sh

Using cgroups Manually
====

cat /proc/self/cgroup
exit
cd /
umount /cgroup/
Charlie/ cpuset/  
umount /cgroup/cpuset/
umount /cgroup/
 


Review
====



End Of Lecture 
=====

Questions?

