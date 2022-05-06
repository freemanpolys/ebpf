# Start ubuntu vm with multipass
```
multipass launch --name ubuntu --mem 2G --disk 10G --cpus 2 impish
```
# Mount workdir into the vm
On mac, permissions need to be given to multipass on [disk](https://github.com/canonical/multipass/issues/1389)
```
multipass mount ./ ubuntu:/ebpf-workdir
```
