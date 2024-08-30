# lindroid roms for Redmi K60/POCO F5 Pro
# changelog
# 2024/09/07
## Upload EvolutionX
# 2024/08-29
## Upload Matrixx
# 2024/08-30
## Upload GKI without KernelSu  
## Upload RisingOS
Note that starting from this RisingOS build, SELinux and hw overlay are disabled with some HACK to make lindroid work by default.  
Please also view patches below
#### 0001-disable-selinux.patch ####
#### 0001-revert-refresh-rate-6-commit-and-disable-hw-overlay.patch ####
## Update mount points
#### mount `/dev/dma_heap` instead of `/dev/ion` to avoid `Aborted` and to make freedreno work
#### also mount `/sdcard` `/storage` `/data`
patch:
#### 0001-mount-dev-dma_heap-instead-of-dev-ion-also-mount-sdc.patch ####
