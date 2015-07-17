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

Or, a super chroot with resource limiting and accounting.



Why?
====

Partially for deployment security and simplification.

Partially for multi-tenancy hosting situations.

With containers you deploy groups of processes in one move.

Immutable deployments where you change nothing.



How it Works 1: chroot
====

The first level of understanding is the chroot API.  Python example:

    import os
    uid = os.getuid()
    gid = os.getgid()
    os.chdir("/tmp")
    os.chroot("/tmp")
    os.setgroups([])
    os.setgid(gid)
    os.setuid(uid)
    os.umask(077)
    os.listdir("/")



chroot
====

This has been standard good process hygene for years.

Change root to a safe directory, drop privilege, force secure files.



How it Works 2: cgroups
====

What about limiting the amount of CPU, disk I/O, and accounting?

That's what cgroups does.



cgroups
====

You can do the following with cgroups today:

* Resource limitation limiting memory, file, cache.
* Prioritization of CPU, disk I/O.
* Accounting to measure resource usage.
* Control to freeze a group, restart it, checkpointing.




cgroups Documentation
====

The official docs seem to be here:

https://www.kernel.org/doc/Documentation/cgroups/cgroups.txt

These may be old given the current state of cgroups.



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



Terrifying Right?
====

1. This is completely different if you use systemd.
2. Each distro also does this differently.
3. ArchLinux apparently can't even do this.
4. Not even getting into namespaces and kernfs changes.
5. It is too complicated for mere mortals.



How it Works 3: namespaces
====

While cgroups control how much something gets, namespaces provide more isolation.

Kind of like super chroot and super fork combined.



Namespaces
====

* The PID namespace for process IDs.
* Network namespace for network interfaces.
* "UTS" namespace to fake change the hostname.
* Mount namespace for fake filesystem layouts.
* IPC namespace to separate the System V IPC mechanisms.
* User namespace separate user IDs.



Namespaces with clone
====

The clone system call implements the most of namespaces.

Compared to cgropus namespaces is pretty nice....if you know C.



Example C Code
====
    static char child_stack[1024 * 1024];

    int main()
    {
        // point at the bottom of stack
        char *stack_start = child_stack + sizeof(child_stack);
        // clone to a function
        int child_pid = clone(child_main, stack_start, 
            CLONE_NEWUTS | SIGCHLD, NULL);
        // wait for it
        waitpid(child_pid, NULL, 0);
        return 0;
    }



Example C Code
====

    int child_main(void* arg)
    {
        // what to run
        char *args[] = {"/bin/bash", NULL};
        // example of UTS host namespace
        sethostname("incontainer", 12);
        // exec bash
        execv(args[0], args);
        // shouldn't reach this point
        return 1;
    }



    
Section 2: Enter Docker
====

Docker provides all of this for you without any of the 
headaches of knowing how cgroups and namespaces work.

Think of Docker as "gcc for deployments" or "gcc to Chef's Ruby".



Docker's Advantage
====

With Docker you combine crafting deployments (Chef/Puppet) with 
process management, isolation, and accounting in one technology.

Physical infrastructure then becomes dumb resources and easier
to manage.



Docker's Design
====

1. A server process runs as root and controls all containers.
2. This process is controlled by a command line tool (docker).
3. You craft images using Dockerfiles (think Makefiles) to indicate what must go in the image.
4. The process then manages containers, which are images that it's executed.



Docker's Design as Linux
====

Another way to look at it is:

* docker server == process 0
* docker cli == your shell
* images == statically compiled binaries
* containers == running processes



Docker Sequence Diagram
====

[Insert diagram of simple docker command sequence]



DockerHub
====

The last piece of information we'll cover is:

https://hub.docker.com/

It's github for docker images.



DockerHub Advantages
====

You can reuse other people's docker images to base yours on.

Post your images to DockerHub and distribute them to others.




Next Lecture
====

This was a quick deep dive into docker, containers, and other tech, so the next 
lesson we'll actually use Docker and Google Computer Engine to make and run an
image.



End Of Lecture 
=====

Questions?

