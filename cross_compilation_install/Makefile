X86_64_INC_DIR             = /usr/x86_64-w64-mingw32/include
X86_64_LIB_DIR             = /usr/x86_64-w64-mingw32/lib
X86_64_BIN_DIR             = /usr/x86_64-w64-mingw32/bin

I686_INC_DIR               = /usr/i686-w64-mingw32/include
I686_LIB_DIR               = /usr/i686-w64-mingw32/lib
I686_BIN_DIR               = /usr/i686-w64-mingw32/bin

X86_64_GCC                 = /usr/bin/x86_64-w64-mingw32-gcc
X86_64_GPP                 = /usr/bin/x86_64-w64-mingw32-g++
X86_64_SDL                 = $(X86_64_INC_DIR)/SDL
X86_64_PTHREAD             = $(X86_64_INC_DIR)/pthread.h

I686_GCC                   = /usr/bin/i686-w64-mingw32-gcc
I686_GPP                   = /usr/bin/i686-w64-mingw32-g++
I686_SDL                   = $(I686_INC_DIR)/SDL
I686_PTHREAD               = $(I686_INC_DIR)/pthread.h

SRC_SDL_X86_64_DIR         = SDL/SDL-1.2-20111107-win64
SRC_SDL_I686_DIR           = SDL/SDL-devel-1.2.15-mingw32

SRC_PTHREAD_X86_64_inc_DIR = pthreads/mingw64/pthreads-w64/x86_64-w64-mingw32/include
SRC_PTHREAD_X86_64_LIB_DIR = pthreads/mingw64/pthreads-w64/x86_64-w64-mingw32/lib
SRC_PTHREAD_X86_64_BIN_DIR = pthreads/mingw64/pthreads-w64/bin
SRC_PTHREAD_I686_INC_DIR   = pthreads/mingw32/pthreads-w32/i686-w64-mingw32/include
SRC_PTHREAD_I686_LIB_DIR   = pthreads/mingw32/pthreads-w32/i686-w64-mingw32/lib
SRC_PTHREAD_I686_BIN_DIR   = pthreads/mingw32/pthreads-w32/bin

###############################################################################
#
###############################################################################
all: x86_64 i686 sdl pthreads
	@echo "installation complete"

x86_64 :
	@if [ ! -a $(X86_64_GCC) ] then sudo apt-get install gcc-mingw-w64-x86-64 fi
	@if [ ! -a $(X86_64_GPP) ] then sudo apt-get install g++-mingw-w64-x86-64 fi

i686 :
	@if [ ! -a $(I686_GCC) ] then sudo apt-get install gcc-mingw-w64-i686 fi
	@if [ ! -a $(I686_GPP) ] then sudo apt-get install g++-mingw-w64-i686 fi

sdl :
	@if [ ! -d $(X86_64_SDL) ] then cd $(SRC_SDL_X86_64_DIR); sudo make cross fi
	@if [ ! -d $(I686_SDL)   ] then cd $(SRC_SDL_I686_DIR);   sudo make cross fi

pthreads :
	@if [ ! -a $(X86_64_PTHREAD) ] then \
		sudo cp $(SRC_PTHREAD_X86_64_INC_DIR)/* $(X86_64_INC_DIR) \
		sudo cp $(SRC_PTHREAD_X86_64_LIB_DIR)/* $(X86_64_LIB_DIR) \
		sudo cp $(SRC_PTHREAD_X86_64_BIN_DIR)/* $(X86_64_BIN_DIR) \
	fi
	@if [ ! -a $(I686_PTHREAD) ] then \
		sudo cp $(SRC_PTHREAD_I686_INC_DIR)/* $(I686_INC_DIR) \
		sudo cp $(SRC_PTHREAD_I686_LIB_DIR)/* $(I686_LIB_DIR) \
		sudo cp $(SRC_PTHREAD_I686_BIN_DIR)/* $(I686_BIN_DIR) \
	fi