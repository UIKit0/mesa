TOP = ../../../..
include $(TOP)/configs/current

LIBNAME = radeonsi_dri.so

PIPE_DRIVERS = \
	$(TOP)/src/gallium/drivers/radeonsi/libradeonsi.a \
	$(TOP)/src/gallium/state_trackers/dri/drm/libdridrm.a \
	$(TOP)/src/gallium/winsys/radeon/drm/libradeonwinsys.a \
	$(TOP)/src/gallium/drivers/trace/libtrace.a \
	$(TOP)/src/gallium/drivers/rbug/librbug.a \
	$(TOP)/src/gallium/drivers/noop/libnoop.a

C_SOURCES = \
	target.c \
	$(COMMON_GALLIUM_SOURCES) \
	$(DRIVER_SOURCES)

DRIVER_DEFINES = \
	-DGALLIUM_RBUG -DGALLIUM_TRACE -DGALLIUM_NOOP

include ../Makefile.dri

GALLIUM_DRI_LIB_DEPS += -ldrm_radeon

symlinks:
