
linux/0002-revert-drm-plane-32b-mask-to-fix-weston-mouse-crash.patch

from https://github.com/YeicoCom/raspberrypi-linux/tree/yeico

The issue appears to be specific to kernel version 6.6.63 after upgrading from 6.6.22 / 6.6.36.

https://github.com/raspberrypi/linux/commit/8181e682d6f4ef209845ec24f0a1eb37764d6731 (culprit)
https://github.com/agherzan/meta-raspberrypi/pull/1392 (kernel bump)
https://github.com/agherzan/meta-raspberrypi/pull/1400 (kernel bump)

Linux bumps
6.6.74 to 6.12.47 https://github.com/nerves-project/nerves_system_rpi4/commit/e58b049cc94f054ced6f64bd695ff0903f142fd1
6.6.65 to 6.6.74 https://github.com/nerves-project/nerves_system_rpi4/commit/9147f56bcd5ee8ec6f1ffc44b5fd574fdc0df8cc
6.6.53 to 6.6.65 https://github.com/nerves-project/nerves_system_rpi4/commit/539c029e86176f2e942e3defbcd1c071da31d4ed

Revert patch
https://github.com/YeicoCom/raspberrypi-linux/commit/bcf13b1972e207b9345ac04b08c4b692769e4d95
https://github.com/YeicoCom/raspberrypi-linux/commit/bcf13b1972e207b9345ac04b08c4b692769e4d95.patch

6.12 does not have that applied

https://github.com/torvalds/linux/blob/adc218676eef25575469234709c2d87185ca223a/drivers/gpu/drm/drm_atomic.c

Solved with a reverse patch.
