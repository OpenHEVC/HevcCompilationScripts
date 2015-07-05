###############################################################################
# check source directory
###############################################################################
ifndef OPEN_HEVC_BUILD_DIR
$(error OPEN_HEVC_BUILD_DIR is not set)
else
ifeq (,$(wildcard $(OPEN_HEVC_BUILD_DIR) ))
$(error $(OPEN_HEVC_BUILD_DIR) not found)
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
# set global variable
###############################################################################
CONFIGURE_GPAC   = ./configure

###############################################################################
#
###############################################################################
all: build_openHevc build_ffmpeg_linux build_gpac


###############################################################################
#
###############################################################################
build_openHevc: 
	cd $(OPEN_HEVC_BUILD_DIR); sudo make uninstall; make clean; make -j 4; sudo make install

build_ffmpeg_linux:
	cd ../ffmpeg; make linux

build_gpac:
	cd $(GPAC_DIR); sudo make uninstall; make distclean; $(CONFIGURE_GPAC); make -j 4; sudo make install
#	sudo rm /usr/local/lib/gpac/gm_ffmpeg_in.so
