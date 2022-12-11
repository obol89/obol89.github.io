---
hide:
    - footer
tags:
    - LVM
    - Preseed
    - Partman
    - Debian
---
# How to create partman LVM partitions on Debian with preseed

Example, how to write a template for Debian to create LVM partitions with partman. It is a bit tricky, as first we have to create boot partition, then virtual group, and further we are able to create partitions by assigning them to volume group.

``` bash
d-i partman-auto/expert_recipe string                 \
      root ::                                         \
            300 512 2048 ext4                         \
                method{ format } format{ }            \
                $primary{ } $bootable{ }              \
                use_filesystem{ } filesystem{ ext4 }  \
                mountpoint{ /boot }                   \
            .                                         \
            2048 20480 -1 xfs                         \
                method{ lvm }                         \
                device{ /dev/sda }                    \
                vg_name{ vg00 }                       \
            .                                         \
            512 1024 2048 linux-swap                  \
                method{ swap } format{ }              \
                $lvmok{ }                             \
                in_vg{ vg00 }                         \
                lv_name{ swap }                       \
            .                                         \
            10240 20480 20480 xfs                     \
                method{ format } format{ }            \
                $lvmok{ }                             \
                lv_name{ root }                       \
                in_vg{ vg00 }                         \
                use_filesystem{ } filesystem{ xfs }   \
                mountpoint{ / }                       \
            .                                         \
            10240 20480 20480 xfs                     \
                method{ format } format{ }            \
                $lvmok{ }                             \
                lv_name{ var }                        \
                in_vg{ vg00 }                         \
                use_filesystem{ } filesystem{ xfs }   \
                mountpoint{ /var }                    \
            .                                         \
            10240 20480 20480 xfs                     \
                method{ format } format{ }            \
                $lvmok{ }                             \
                lv_name{ opt }                        \
                in_vg{ vg00 }                         \
                use_filesystem{ } filesystem{ xfs }   \
                mountpoint{ /opt }                    \
            .                                         \
            10240 20480 -1 xfs                        \
                method{ format } format{ }            \
                $lvmok{ }                             \
                lv_name{ home }                       \
                in_vg{ vg00 }                         \
                use_filesystem{ } filesystem{ xfs }   \
                mountpoint{ /home }                   \
            .                                         \
```
