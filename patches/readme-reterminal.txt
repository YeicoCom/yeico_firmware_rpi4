got src from https://github.com/YeicoCom/seeed-linux-dtoverlays/tree/master/modules/mipi_dsi

from within ./shell.sh

rm ~/yeico_firmware_rpi4/linux/0003-panel-reterminal-mipi_dsi.patch
make linux-dirclean
make linux-patch
cp -fr build/linux-custom build/linux-patched
cp -fr ~/yeico_firmware_rpi4/patches/reterminal/* build/linux-custom/drivers/gpu/drm/panel/
(cd build && diff -ruN linux-patched linux-custom > ~/yeico_firmware_rpi4/linux/0003-panel-reterminal-mipi_dsi.patch)
cat ~/yeico_firmware_rpi4/linux/0003-panel-reterminal-mipi_dsi.patch

make linux-dirclean
make linux-rebuild
make

./artifac.sh
