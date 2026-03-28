---
comments: true
hide:
    - footer
tags:
    - LVM
    - Preseed
    - Partman
    - Debian
---
# Create partman LVM partitions on Debian with preseed

Preseed template for Debian LVM partitions with partman. The boot partition must be created first, then the volume group, then individual logical volumes assigned to that group.

```text
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
