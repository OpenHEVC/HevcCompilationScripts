###############################################################################
# check source directory
###############################################################################
ifndef FFMPEG_DIR
$(error FFMPEG_DIR is not set)
else
ifeq (,$(wildcard $(FFMPEG_DIR) ))
$(error $(FFMPEG_DIR) not found)
endif
endif

ifndef VLC_DIR
$(error VLC_DIR is not set)
else
ifeq (,$(wildcard $(VLC_DIR) ))
$(error $(VLC_DIR) not found)
endif
endif

###############################################################################
# set global variable
###############################################################################
ifeq ($(WIN_OS), w32)
WIN_OS          = w32
ARCH            = i686
VLC_ZIP         = vlc-2.2.0-git-win32
else
WIN_OS          = w64
ARCH            = x86_64
VLC_ZIP         = vlc-2.2.0-git-win64
endif

WIN_OS_DIR      = vlc_$(WIN_OS)
DROPBOX_WIN_OS  = $(DROPBOX_DIR)/HEVC/DLL/$(WIN_OS_DIR)

PREFIX          = $(ARCH)-w64-mingw32

CONFIGURE_LINUX = ./configure --disable-lua --disable-mad --disable-a52 --disable-libgcrypt
CONFIGURE_WIN   = ../extras/package/win32/configure.sh --host=$(PREFIX) --disable-chromaprint --disable-zlib

###############################################################################
# set compile variable
###############################################################################

###############################################################################
#
###############################################################################
all: 
	@echo "usage :"
	@echo "make linux or make w32 or make w64"

w32: 
	make build_win WIN_OS=w32

w64: 
	make build_win WIN_OS=w64

linux:
	make build_linux

###############################################################################
# build vlc
###############################################################################
build_win: update_vlc_win build_ffmpeg_win 
	cd $(VLC_DIR)/$(WIN_OS_DIR)/win32; $(CONFIGURE_WIN); make -j 4; make package-win32-zip
ifneq (,$(wildcard $(DROPBOX_WIN_OS) ))
	cd $(VLC_DIR)/$(WIN_OS_DIR)/win32; cp -f $(VLC_ZIP).zip $(DROPBOX_WIN_OS)/$(VLC_ZIP)_$(shell date +"%d-%m-%y").zip
endif

build_linux:update_vlc_linux build_ffmpeg_linux 
	cd $(VLC_DIR)/vlc_linux; export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/; make distclean; $(CONFIGURE_LINUX); make -j 4

###############################################################################
# get vlc source
###############################################################################
update_vlc_win:
ifeq (,$(wildcard $(VLC_DIR)/$(WIN_OS_DIR) ))
	cd $(VLC_DIR); git clone git://git.videolan.org/vlc.git $(WIN_OS_DIR)
	cd $(VLC_DIR)/$(WIN_OS_DIR); mkdir -p contrib/win32; cd contrib/win32
ifeq ($(WIN_OS), w64)
	ln -s $(PWD)/vlc-contrib-$(PREFIX)-20130408.tar.bz2 $(VLC_DIR)/$(WIN_OS_DIR)/contrib/win32/vlc-contrib-$(PREFIX)-latest.tar.bz2
endif
	cd $(VLC_DIR)/$(WIN_OS_DIR)/contrib/win32; ../bootstrap --host=$(PREFIX); make prebuilt
	cd $(VLC_DIR)/$(WIN_OS_DIR); ./bootstrap; mkdir win32
endif

update_vlc_linux:
ifeq (,$(wildcard $(VLC_DIR)/vlc_linux ))
	cd $(VLC_DIR); git clone git://git.videolan.org/vlc.git vlc_linux
	cd $(VLC_DIR)/vlc_linux; ./bootstrap
endif

###############################################################################
# build ffmpeg
###############################################################################
build_ffmpeg_win:
	cd ../ffmpeg; make $(WIN_OS)
	cd $(FFMPEG_DIR); make install

build_ffmpeg_linux:
	cd ../ffmpeg; make linux