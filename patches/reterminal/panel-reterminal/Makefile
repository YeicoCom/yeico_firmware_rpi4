
obj-m := mipi_dsi.o
mipi_dsi-objs := mipi_dsi_drv.o touch_panel.o panel-ili9881x.o

all:
	$(MAKE) -C $(KDIR) M=$(PWD) modules
	$(MAKE) -C $(KDIR) M=$(PWD) modules_install

clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean

