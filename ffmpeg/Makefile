###############################################################################
# check source directory
###############################################################################
ifndef OPEN_HEVC_DIR
$(error OPEN_HEVC_DIR is not set)
else
ifeq (,$(wildcard $(OPEN_HEVC_DIR) ))
$(error $(OPEN_HEVC_DIR) not found)
endif
endif

ifndef FFMPEG_DIR
$(error FFMPEG_DIR is not set)
else
ifeq (,$(wildcard $(FFMPEG_DIR) ))
$(error $(FFMPEG_DIR) not found)
endif
endif

###############################################################################
# set global variable
###############################################################################
ifeq ($(WIN_OS), w32)
WIN_OS          = w32
ARCH            = i686
else
WIN_OS          = w64
ARCH            = x86_64
endif

WIN_OS_DIR      = ffmpeg_$(WIN_OS)
ifeq ($(SHVC), 1)
BIN_DIR         = $(OPEN_HEVC_DIR)/bin/shvc/$(WIN_OS_DIR)
DROPBOX_WIN_OS  = $(DROPBOX_DIR)/SHVC/DLL/$(WIN_OS_DIR)
else
BIN_DIR         = $(OPEN_HEVC_DIR)/bin/$(WIN_OS_DIR)
DROPBOX_WIN_OS  = $(DROPBOX_DIR)/HEVC/DLL/$(WIN_OS_DIR)
endif

PREFIX          = $(ARCH)-w64-mingw32

ifdef VLC_DIR
ifneq (,$(wildcard $(VLC_DIR)/vlc_$(WIN_OS) ))
INSTALL_VLC_DIR = --prefix=$(VLC_DIR)/vlc_$(WIN_OS)/contrib/$(PREFIX)
endif
endif

CONFIGURE_LINUX = ./configure --disable-static --enable-shared
CONFIGURE_WIN   = ./configure $(INSTALL_VLC_DIR) --enable-cross-compile --enable-w32threads --cross-prefix=$(PREFIX)- --arch=$(ARCH) --target-os=mingw32 --enable-memalign-hack --disable-shared --enable-static --disable-debug --enable-runtime-cpudetect --disable-zlib

###############################################################################
# set compile variable
###############################################################################
CC              = $(PREFIX)-gcc
CFLAGS          = -I. -I"$(FFMPEG_DIR)/" -I"$(OPEN_HEVC_DIR)/gpac/modules/openhevc_dec/" -I"$(OPEN_HEVC_DIR)/main_hm/" -I"/usr/$(PREFIX)/include/SDL" -DPIC -Wl,-Bsymbolic -fno-tree-vectorize
LDFLAGS_DLL     = -L"/usr/$(PREFIX)/lib/" -L. -lavdevice -lavfilter -lavformat -lavcodec -lswscale -lavutil -lswresample -lws2_32 -lm
LDFLAGS_EXEC    = -L"/usr/$(PREFIX)/lib/" -L. -lSDL  -lavformat -lavcodec -lavutil -lws2_32

OBJ             = sdl.obj getopt.obj main.obj
DLL             = libLibOpenHevcWrapper.dll
DEF             = libLibOpenHevcWrapper.def
EXEC            = hevc_$(WIN_OS_DIR).exe

###############################################################################
#
###############################################################################
all: 
	@echo "usage :"
	@echo "make linux or make w32 or make w64 or shvc_w64"

w32: 
	make build_dll WIN_OS=w32 SHVC=0

w64: 
	make build_dll WIN_OS=w64 SHVC=0

shvc_w64: 
	make build_dll WIN_OS=w64 SHVC=1

linux:
	make build_linux SHVC=0

###############################################################################
# build_dll
###############################################################################
build_dll: build_win input_copy $(DLL) $(EXEC)
	cp -f $(DLL)  $(WIN_OS_DIR)/$(DLL)
	cp -f $(DEF)  $(WIN_OS_DIR)/$(DEF)
	cp -f $(EXEC) $(WIN_OS_DIR)/$(EXEC)
	cp -f $(FFMPEG_DIR)/ffmpeg.exe $(WIN_OS_DIR)/ffmpeg.exe
	cp -f $(WIN_OS_DIR)/* $(BIN_DIR)/.
ifneq (,$(wildcard $(DROPBOX_WIN_OS) ))
	cp -f $(WIN_OS_DIR)/* $(DROPBOX_WIN_OS)/.
endif

input_copy: clean
	cp -f $(FFMPEG_DIR)/libavcodec/libavcodec.a .
	cp -f $(FFMPEG_DIR)/libavdevice/libavdevice.a .
	cp -f $(FFMPEG_DIR)/libavfilter/libavfilter.a .
	cp -f $(FFMPEG_DIR)/libavformat/libavformat.a .
	cp -f $(FFMPEG_DIR)/libavutil/libavutil.a .
	cp -f $(FFMPEG_DIR)/libswscale/libswscale.a .
	cp -f $(FFMPEG_DIR)/libswresample/libswresample.a .
	cp -f $(WIN_OS_DIR)/SDL.dll .

###############################################################################
# build ffmpeg
###############################################################################
build_win:
	cd $(FFMPEG_DIR); make distclean; $(CONFIGURE_WIN); make -j 4

build_linux:
	cd $(FFMPEG_DIR); sudo make uninstall; make distclean; $(CONFIGURE_LINUX); make -j 4; sudo make install


###############################################################################
# libLibOpenHevcWrapper.dll
###############################################################################
$(DLL):  openHevcWrapper.obj
	$(CC) -shared -o $@ $< $(LDFLAGS_DLL)
	echo EXPORTS > $(DEF)
ifeq ($(WIN_OS), w32)
	nm $(DLL) | grep ' T _libOpenHevc' | sed 's/.* T _//' >> $(DEF)
else
	nm $(DLL) | grep ' T libOpenHevc' | sed 's/.* T //' >> $(DEF)
endif


openHevcWrapper.obj: $(OPEN_HEVC_DIR)/gpac/modules/openhevc_dec/openHevcWrapper.c
	$(CC) -o $@ -c $< $(CFLAGS)

###############################################################################
# hevc_$(WIN_OS_DIR).exe
###############################################################################
$(EXEC): $(DLL) $(OBJ)
	$(CC) -o $@ $^ $(LDFLAGS_EXEC)

sdl.obj: $(OPEN_HEVC_DIR)/main_hm/sdl.c
	$(CC) -o $@ -c $< $(CFLAGS)
getopt.obj: $(OPEN_HEVC_DIR)/main_hm/getopt.c
	$(CC) -o $@ -c $< $(CFLAGS)
main.obj: $(OPEN_HEVC_DIR)/main_hm/main.c
	$(CC) -o $@ -c $< $(CFLAGS)

###############################################################################
# clean
###############################################################################
clean_all: clean
	rm -f ffmpeg_w32/*.exe ffmpeg_w32/$(DLL) ffmpeg_w32/$(DEF)
	rm -f ffmpeg_w64/*.exe ffmpeg_w64/$(DLL) ffmpeg_w64/$(DEF)
clean:
	rm -f *.obj *.a *~ *.exe *.dll *.def
