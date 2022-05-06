# eBPF packages on Ubuntu 21.10
// for bpftool command 
sudo apt install  linux-tools-common

sudo bpftool btf dump file /sys/kernel/btf/vmlinux format c > vmlinux.h

WARNING: bpftool not found for kernel 5.13.0-35

  You may need to install the following packages for this specific kernel:
    linux-tools-5.13.0-35-generic
    linux-cloud-tools-5.13.0-35-generic

  You may also want to install one of the following packages to keep up to date:
    linux-tools-generic
    linux-cloud-tools-generic

sudo apt install   linux-tools-5.13.0-35-generic  linux-cloud-tools-5.13.0-35-generic

# ebpf-hello-world

This repo contains two simple example eBPF applications made to acoompany Sartura's eBPF programming [blog post](https://www.sartura.hr/blog/simple-ebpf-core-application/).

The first program is a hello world program that prints a message to `/sys/kernel/debug/tracing/trace_pipe`.
The second demonstrates BPF map use and execve system call tracing by storing the command name, UID and PID of the collected execve event.
## libbpf library
No need to build libbpf from source. It can be find at /usr/lib/x86_64-linux-gnu/libbpf.a

## Building

A short version of the build steps is provided here:
```
$ bpftool btf dump file /sys/kernel/btf/vmlinux format c > vmlinux.h
$ clang -g -O2 -target bpf -D__TARGET_ARCH_x86_64 -I . -c hello.bpf.c -o hello.bpf.o
$ bpftool gen skeleton hello.bpf.o > hello.skel.h
$ clang -g -O2 -Wall -I . -c hello.c -o hello.o
$ clang -Wall -O2 -g hello.o /usr/lib/x86_64-linux-gnu/libbpf.a -lelf -lz -o hello
$ sudo ./hello
```
