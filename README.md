# komake

Komake is a wrapper of make(1) that limits the concurrency of makes being recursively spawned.

## Synopsis

```
$ komake                   # runs `make all`
$ komake -j4 myproj        # options are passsed to `make`
$ MAKE=my-make komake      # use my favorite make, wrapped by komake
```

## Description

While working on large projects, in order to reduce build time, it is generally preferable to use the `-j` option so that make (1) spawns multiple sub-processes concurrently. When there are sub-projects, the main Makefile is typically written so that sub-processes, each running make (1), will be spawned 
for building each of those sub-projects.

However, when there is a diamond dependency between those sub-projects (e.g., sub-project-1 and sub-project-2 depending on sub-project-3), make (1) does not have the capability of limiting concurrent execution of `make sub-project-3`. When race happens, the result of the build process becomes unpredictable, often ending up in an error.

Komake is a wrapper for make (1) that resolves this problem, by limiting the concurrency of make (1) building sub-projects to exactly one.

## Setting Concurrency

In order to achieve the necessary concurrency, users are advised to set the argument of `-j` option to the number of CPU cores plus the number of make (1) processes that may be invoked. This is because make (1) processes being delayed due to the concurrency limitation counts towards the concurrency as well.

## Notes

While komake is meant to work, Japanese users should not assume that you can win by using komake. Read the name carefully.
