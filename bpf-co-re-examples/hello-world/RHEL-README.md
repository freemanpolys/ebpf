# Prerequisite :
https://github.com/libbpf/libbpf-bootstrap
https://github.com/sartura/ebpf-hello-world

https://facebookmicrosites.github.io/bpf/blog/2020/02/20/bcc-to-libbpf-howto-guide.html
https://www.sartura.hr/blog/simple-ebpf-core-application/
 
https://www.sartura.hr/blog/ebpf-development-and-integration-with-replica-one/
# Check if vmlinux exist

First, we must verify that the kernel is built with BTF info, which can be done by enabling kernel config options CONFIG_DEBUG_INFO=y and CONFIG_DEBUG_INFO_BTF=y.

 

ls /sys/kernel/btf/vmlinux

 

# Install bpftool

yum install bpftool libbpf-devel

 

# Generate vmlinux.h

bpftool btf dump file /sys/kernel/btf/vmlinux format c > vmlinux.h

BCC vs BPF-CORE : https://facebookmicrosites.github.io/bpf/blog/2020/02/20/bcc-to-libbpf-howto-guide.html

 

# Build
git clone https://github.com/libbpf/libbpf
clang -g -O2 -target bpf -D__TARGET_ARCH_x86_64 -I . -c hello.bpf.c -o hello.bpf.o

 

 

# Errors

fatal error: 'bpf/bpf_helpers.h' file not found

 

 

#BPF helpers :

https://www.man7.org/linux/man-pages/man7/bpf-helpers.7.html

For our sample application, the bpf_helpers header defines the SEC macro, which in turn places the function below into a special ELF section to let libbpf know where to attach the BPF program. The bpf_helpers.h header comes included with libbpf

 

 

We can see that the SEC macro specifies a path, SEC("tracepoint/syscalls/sys_enter_execve"). If we look at the /sys/kernel/debug/tracing/events directory, we can see that the syscalls/sys_enter_execve parts match. A simple way to find a matching event to attach the BPF program is to look in the /sys/kernel/debug/tracing directory. In other words, this will attach the BPF program to the execve syscall and trigger the BPF program on system call entry.
