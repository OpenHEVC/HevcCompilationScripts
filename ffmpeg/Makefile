ifeq ($(WIN_OS), w32)
WIN_OS           = w32
ARCH             = i686
else
WIN_OS           = w64
ARCH             = x86_64
endif

WIN_OS_DIR       = ffmpeg_$(WIN_OS)
BIN_DIR          = $(OPEN_HEVC_DIR)/bin/$(WIN_OS_DIR)
DROPBOX_WIN_OS   = $(DROPBOX_DIR)/HEVC/DLL/$(WIN_OS_DIR)

PREFIX           = $(ARCH)-w64-mingw32
CONFIGURE_FFMPEG = ./configure --enable-cross-compile --enable-w32threads --cross-prefix=$(PREFIX)- --arch=$(ARCH) --target-os=mingw32 --enable-memalign-hack --disable-shared --enable-static --disable-debug --enable-runtime-cpudetect --disable-zlib

###############################################################################
#
###############################################################################
CC           = $(PREFIX)-gcc
CFLAGS       = -I. -I"$(FFMPEG_DIR)/" -I"$(OPEN_HEVC_DIR)/gpac/modules/openhevc_dec/" -I"$(OPEN_HEVC_DIR)/main_hm/" -I"/usr/$(PREFIX)/include/SDL" -DPIC -Wl,-Bsymbolic -fno-tree-vectorize
LDFLAGS_DLL  = -L"/usr/$(PREFIX)/lib/" -L. -lavdevice -lavfilter -lavformat -lavcodec -lswscale -lavutil -lws2_32 -lm
LDFLAGS_EXEC = -L"/usr/$(PREFIX)/lib/" -L. -lSDL  -lavformat -lavcodec -lavutil -lws2_32

OBJ          = sdl.obj getopt.obj main.obj
DLL          = libLibOpenHevcWrapper.dll
DEF          = libLibOpenHevcWrapper.def
EXEC         = hevc_$(WIN_OS_DIR).exe

###############################################################################
#
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
#
###############################################################################
all: 
	@echo "usage :"
	@echo "make w32 or make w64"

w32: 
	make build_dll WIN_OS=w32

w64: 
	make build_dll WIN_OS=w64

###############################################################################
#
###############################################################################
build_dll: build_ffmpeg input_copy $(DLL) $(EXEC)
	cp -f $(DLL)  $(WIN_OS_DIR)/$(DLL)
	cp -f $(DEF)  $(WIN_OS_DIR)/$(DEF)
	cp -f $(EXEC) $(WIN_OS_DIR)/$(EXEC)
	cp -f $(FFMPEG_DIR)/ffmpeg.exe $(WIN_OS_DIR)/ffmpeg.exe
	cp -f $(WIN_OS_DIR)/* $(BIN_DIR)/.
	@if [ -d $(DROPBOX_WIN_OS) ]; then \
		cp -f $(WIN_OS_DIR)/* $(DROPBOX_WIN_OS)/. ; \
	fi

input_copy: clean
	cp -f $(FFMPEG_DIR)/libavcodec/libavcodec.a .
	cp -f $(FFMPEG_DIR)/libavdevice/libavdevice.a .
	cp -f $(FFMPEG_DIR)/libavfilter/libavfilter.a .
	cp -f $(FFMPEG_DIR)/libavformat/libavformat.a .
	cp -f $(FFMPEG_DIR)/libavutil/libavutil.a .
	cp -f $(FFMPEG_DIR)/libswscale/libswscale.a .
	cp -f $(WIN_OS_DIR)/SDL.dll .

build_ffmpeg:
	cd $(FFMPEG_DIR); make distclean; $(CONFIGURE_FFMPEG); make -j 4

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
