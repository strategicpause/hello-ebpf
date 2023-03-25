# hello-ebpf

## Setup
~~~
sudo dnf install bpftool clang libbpf-devel
~~~

## Build
~~~
$ bpftool btf dump file /sys/kernel/btf/vmlinux format c > vmlinux.h
$ clang -g -O2 -target bpf -D__TARGET_ARCH_x86_64 -I . -c hello.bpf.c -o hello.bpf.o
$ bpftool gen skeleton hello.bpf.o > hello.skel.h


$ clang -g -O2 -Wall -I . -c hello.c -o hello.o
$ clang -Wall -O2 -g hello.o /usr/src/kernels/6.2.7-200.fc37.x86_64/tools/bpf/resolve_btfids/libbpf/libbpf.a -lelf -lz -o hello
~~~

## Run
~~~
sudo ./hello
           <...>-32919   [005] d..31  8732.858441: bpf_trace_printk: Hello world!
~~~