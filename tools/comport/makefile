CROSS_COMPILE =
ARCH= L350
${ARCH}:=${ARCH}

MAJOR=1
MINOR=0
REVER=0
SVNUP=1

CC=gcc
AR=ar
RANLIB=ranlib
STRIP=strip

# ==================================
# ** ARCH SETTINGS FOR L100, L200 **
# ==================================

TMP=$(ARCH)
ifneq (,$(filter L200 L100,$(TMP)))
    CROSS_COMPILE=/usr/local/arm_tools_3.3.4/bin/arm-elf-
	STRIP=flthdr -z -r -s 16384
    CFLAGS+=-DARCH_$(ARCH)
    CFLAGS+=-DUCLINUX -DIS_LITTLE_ENDIAN -Wall \
	-I/usr/local/arm_tools_3.3.4/arm-elf/include \
	-L./ \
	-L/usr/local/arm_tools_3.3.4/arm-elf/lib
	LDFLAGS=-Wl,-elf2flt
endif

# ==================================
# ** ARCH SETTINGS FOR L300, L350 **
# ==================================

TMP=$(ARCH)
ifneq (,$(filter N300 L300 L350,$(TMP)))
	CROSS_COMPILE=arm-linux-
	STRIP = strip
	CFLAGS+=-DARCH_$(ARCH)
	CFLAGS+=-DLINUX -DIS_LITTLE_ENDIAN -Wall -D_GNU_SOURCE -D_REENTRANT -D_LARGE_THREADS -D_THREAD_SAFE
	LDFLAGS=
endif

# ==================================
# ** ARCH SETTINGS FOR LINUX I386 **
# ==================================

TMP=$(ARCH)
ifneq (,$(filter I386,$(TMP)))
	CROSS_COMPILE =
	STRIP = strip
	CFLAGS+=-DARCH_LINUX_$(ARCH)
	CFLAGS+=-DLINUX -DIS_LITTLE_ENDIAN -Wall -D_GNU_SOURCE -D_REENTRANT -D_LARGE_THREADS -D_THREAD_SAFE
	LDFLAGS=
endif

export CC
export AR
export STRIP
export CROSS_COMPILE
export CFLAGS
export LDFLAGS

APP_BINARY_NAME = comport

all: entry version comport

entry: clean
	@clear;

	@echo " ";
	@echo " ===========================";
	@echo " **  comport compilation **";
	@echo " ===========================";
	@echo " ARCH: $(ARCH)";
	@echo " ";

version:
	@echo " "; 
	@echo " ========================================="; 
	@echo " **  Create Software Version Head File  **"; 
	@echo " ========================================="; 
	sh scripts/mkversion ${SVNUP} ${MAJOR} ${MINOR} ${REVER}

comport: $(APP_BINARY_NAME)

	@echo " ";
	@echo " ** Linking... **";
	@echo " ";

	${CROSS_COMPILE}$(CC) $(CFLAGS) -c -o CComPort.o CComPort.c
	${CROSS_COMPILE}$(CC) $(CFLAGS) $(LDFLAGS) -o comport comport.c *.o

	@echo " ";
	@echo " ** Striping... **";
	@echo " ";

	${CROSS_COMPILE}${STRIP} ${APP_BINARY_NAME}

	@echo " ";
	@echo " ** Done **";
	@echo " ";

	@echo " ** Clear temporary files **";

	@rm -f *.gdb *.o

clean:
	@rm -f *.gdb ${APP_BINARY_NAME}

