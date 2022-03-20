# Ensure the VMs use discrete GPUs

## Reference

Post:
<https://communities.vmware.com/t5/VMware-Workstation-Pro/VMware-Workstation-cannot-connect-to-the-virtual-machine-Vmware/m-p/2899524/highlight/false#M174709>

## Ensure dGPU

Read the post (the saved pdf file) for more info.

VMware 16 uses vulkan for rendering, this requires the graphic hardware support, and the embedded GPU from intel doesn't.

therefore, I need to ensure the vm is configured to use the discrete GPU on
the host (in my case, it is GTX 1050 on Dell XPS)

to this end, I need to edit my vm setting file.

the vm's name is `microk8s-worker-aa`, the vm directory is:

`$WORK_ROOT/media/vmware/<vm>`

the setting file is named after the vm with extension `.vmx`

in this file, I need to add 2 key-value pairs (option 2 in the post)

```text
mks.forceDiscreteGPU = "TRUE"
mks.vk.allowUnsupportedDevices = "TRUE"
```

I can also make this a default, global setting by editing the global conf file

```text
/etc/vmware/config for host machine scope
/home/$USER/.vmware/config for user scope
```
