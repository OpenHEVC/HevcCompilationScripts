CONFIGURE_FFMPEG = ./configure --disable-shared --enable-static
CONFIGURE_GPAC   = ./configure
###############################################################################
#
###############################################################################
ifndef OPEN_HEVC_BUILD_DIR
$(error OPEN_HEVC_BUILD_DIR is not set)
else
ifeq (,$(wildcard $(OPEN_HEVC_BUILD_DIR) ))
$(error $(OPEN_HEVC_BUILD_DIR) not found)
endif
endif

ifndef FFMPEG_DIR
$(error FFMPEG_DIR is not set)
else
ifeq (,$(wildcard $(FFMPEG_DIR) ))
$(error $(FFMPEG_DIR) not found)
endif
endif

ifndef GPAC_DIR
$(error GPAC_DIR is not set)
else
ifeq (,$(wildcard $(GPAC_DIR) ))
$(error $(GPAC_DIR) not found)
endif
endif

###############################################################################
#
###############################################################################
all: build_openHevc build_ffmpeg build_gpac

build_openHevc:
	cd $(OPEN_HEVC_BUILD_DIR); sudo make uninstall; make clean; make -j 4; sudo make install
build_ffmpeg:
	cd $(FFMPEG_DIR); sudo make uninstall; make distclean; $(CONFIGURE_FFMPEG); make -j 4; sudo make install
build_gpac:
	cd $(GPAC_DIR); sudo make uninstall; make distclean; $(CONFIGURE_GPAC); make -j 4; sudo make install