.PHONY: compile
CC = gcc
LD = ld

# find the OS
uname_S := $(shell sh -c 'uname -s 2>/dev/null || echo not')

# Compile flags for linux / osx
ifeq ($(uname_S),Linux)
	# gcc -fPIC -std=gnu99 -c -o redismon.o redismon.c
	# ld -o redismon.so redismon.o -shared -Bsymbolic -lc
	SHOBJ_CFLAGS ?= -Wall -W -O3 -fno-common -g -ggdb -std=c99
	SHOBJ_LDFLAGS ?= -shared
else
    # gcc -dynamic -fno-common -std=gnu99 -g -ggdb -c -o redismon.o redismon.c
    # ld -o redismon.so redismon.o -bundle -undefined dynamic_lookup -lc
	SHOBJ_CFLAGS ?=  -dynamic -fno-common -std=gnu99 -g -ggdb
	SHOBJ_LDFLAGS ?= -bundle -undefined dynamic_lookup
endif

compile: clean redismon.so

.SUFFIXES: .c .so .xo .o

.c.xo:
	$(CC) -I. $(CFLAGS) $(SHOBJ_CFLAGS) -fPIC -c $< -o $@

redismon.c: redismodule.h

redismon.so: redismon.xo
	$(LD) -o $@ $<  $(SHOBJ_LDFLAGS) $(LIBS) -lc

clean:
	rm -rf *.xo *.so *.o
