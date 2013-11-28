MINGW_W64_DIR              = /usr/share/mingw-w64

X86_64_DIR                 = /usr/x86_64-w64-mingw32
X86_64_INC_DIR             = $(X86_64_DIR)/include
X86_64_LIB_DIR             = $(X86_64_DIR)/lib
X86_64_BIN_DIR             = $(X86_64_DIR)/bin

I686_DIR                   = /usr/i686-w64-mingw32
I686_INC_DIR               = $(I686_DIR)/include
I686_LIB_DIR               = $(I686_DIR)/lib
I686_BIN_DIR               = $(I686_DIR)/bin

X86_64_GCC                 = /usr/bin/x86_64-w64-mingw32-gcc
X86_64_GPP                 = /usr/bin/x86_64-w64-mingw32-g++
X86_64_PKG                 = /usr/bin/x86_64-w64-mingw32-pkg-config
X86_64_SDL                 = $(X86_64_INC_DIR)/SDL
X86_64_PTHREAD             = $(X86_64_INC_DIR)/pthread.h

I686_GCC                   = /usr/bin/i686-w64-mingw32-gcc
I686_GPP                   = /usr/bin/i686-w64-mingw32-g++
I686_SDL                   = $(I686_INC_DIR)/SDL
I686_PTHREAD               = $(I686_INC_DIR)/pthread.h

SRC_SDL_X86_64_DIR         = SDL/SDL-1.2-20111107-win64
SRC_SDL_I686_DIR           = SDL/SDL-devel-1.2.15-mingw32

SRC_PTHREAD_X86_64_INC_DIR = pthreads/mingw64/pthreads-w64/x86_64-w64-mingw32/include
SRC_PTHREAD_X86_64_LIB_DIR = pthreads/mingw64/pthreads-w64/x86_64-w64-mingw32/lib
SRC_PTHREAD_X86_64_BIN_DIR = pthreads/mingw64/pthreads-w64/bin
SRC_PTHREAD_I686_INC_DIR   = pthreads/mingw32/pthreads-w32/i686-w64-mingw32/include
SRC_PTHREAD_I686_LIB_DIR   = pthreads/mingw32/pthreads-w32/i686-w64-mingw32/lib
SRC_PTHREAD_I686_BIN_DIR   = pthreads/mingw32/pthreads-w32/bin

###############################################################################
#
###############################################################################
all: mingw_w64 x86_64 i686 sdl pthreads
	@echo "installation complete"

mingw_w64:
ifeq (,$(wildcard $(MINGW_W64_DIR) ))
	cd package; sudo dpkg -i *.deb; sudo apt-get install -f
endif
x86_64 :
ifeq (,$(wildcard $(X86_64_GCC) ))
	sudo apt-get install gcc-mingw-w64-x86-64
endif
ifeq (,$(wildcard $(X86_64_GPP) ))
	sudo apt-get install g++-mingw-w64-x86-64
endif

i686 :
ifeq (,$(wildcard $(I686_GCC) ))
	sudo apt-get install gcc-mingw-w64-i686
endif
ifeq (,$(wildcard $(I686_GPP) ))
	sudo apt-get install g++-mingw-w64-i686
endif

sdl :
ifeq (,$(wildcard $(X86_64_SDL) ))
	cd $(SRC_SDL_X86_64_DIR); sudo make cross
endif
ifeq (,$(wildcard $(I686_SDL) ))
	cd $(SRC_SDL_I686_DIR);   sudo make cross
endif

pthreads :
ifeq (,$(wildcard $(X86_64_PTHREAD) ))
	sudo cp $(SRC_PTHREAD_X86_64_INC_DIR)/* $(X86_64_INC_DIR)
	sudo cp $(SRC_PTHREAD_X86_64_LIB_DIR)/* $(X86_64_LIB_DIR)
	sudo cp $(SRC_PTHREAD_X86_64_BIN_DIR)/* $(X86_64_BIN_DIR)
endif
ifeq (,$(wildcard $(I686_PTHREAD) ))
	sudo cp $(SRC_PTHREAD_I686_INC_DIR)/* $(I686_INC_DIR)
	sudo cp $(SRC_PTHREAD_I686_LIB_DIR)/* $(I686_LIB_DIR)
	sudo cp $(SRC_PTHREAD_I686_BIN_DIR)/* $(I686_BIN_DIR)
endif
