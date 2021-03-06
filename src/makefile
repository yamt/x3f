# MAKEFILE
# 
# Copyright (c) 2010 - Roland Karlsson (roland@proxel.se)
# BSD-style - see doc/copyright.txt

# -----------------------------------------------------------
# Usage:
# Write "make" shall build the executables on Mac, Linux or Windows.
# Write "make clean" or "make clobber" cleans up less or more.
# -----------------------------------------------------------

# Set the SYS variable
# -----------------------------------------------------------

  SYS = generic
ifdef COMSPEC
  SYS = windows
else
  OSTYPE = $(shell bash -c 'echo $$OSTYPE')
ifeq (linux-gnu, $(OSTYPE))
  SYS = linux
else
ifeq (darwin, $(findstring darwin,$(OSTYPE)))
  SYS = osx
endif
endif
endif

# Set compilation and load flags etc
# -----------------------------------------------------------

ifeq (windows, $(SYS))
  EXE = .exe
  CFBASE = -O3 -Wall
  LDFLAGS = -lm
else
ifeq (linux, $(SYS))
  EXE =
  CFBASE = -O3 -Wall
  LDFLAGS = -lm
else
ifeq (osx, $(SYS))
  EXE =
  ARCHX = -arch i386 -arch x86_64 -arch ppc
  ifdef PPC64
    # To be able to compile ppc64 on a 10.6 machine
    ARCH = $(ARCHX) -arch ppc64 -mmacosx-version-min=10.5 -isysroot /Developer/SDKs/MacOSX10.5.sdk
  else
    ARCH = $(ARCHX)
  endif
  CFBASE = -O3 -Wall $(ARCH)
  LDFLAGS = -lm $(ARCH)
else
  EXE = 
  CFBASE = -O3 -Wall
  LDFLAGS = -lm
endif
endif
endif

CFLAGS = $(CFBASE) $(C)

BINDIR = ../bin/$(SYS)

# Build dependencies
# -----------------------------------------------------------

all: $(BINDIR)/x3f_extract$(EXE) $(BINDIR)/x3f_io_test$(EXE) $(BINDIR)/x3f_merge$(EXE)

$(BINDIR)/x3f_extract$(EXE): $(BINDIR)/x3f_extract.o $(BINDIR)/x3f_io.o
	gcc $^ -o $@ $(LDFLAGS)

$(BINDIR)/x3f_merge$(EXE): $(BINDIR)/x3f_merge.o $(BINDIR)/x3f_io.o
	gcc $^ -o $@ $(LDFLAGS) 

$(BINDIR)/x3f_io_test$(EXE): $(BINDIR)/x3f_io_test.o $(BINDIR)/x3f_io.o
	gcc $^ -o $@ $(LDFLAGS) 

$(BINDIR)/x3f_extract.o: x3f_extract.c x3f_io.h $(BINDIR)/.dummy
	gcc $(CFLAGS) $< -c -o $@

$(BINDIR)/x3f_merge.o: x3f_merge.c x3f_io.h $(BINDIR)/.dummy
	gcc $(CFLAGS) $< -c -o $@

$(BINDIR)/x3f_io_test.o: x3f_io_test.c x3f_io.h $(BINDIR)/.dummy
	gcc $(CFLAGS) $< -c -o $@

$(BINDIR)/x3f_io.o: x3f_io.c x3f_io.h $(BINDIR)/.dummy
	gcc $(CFLAGS) $< -c -o $@

# This does not work perfect on Windows
# The file x is newer than *.o files. Why?
# So - the first time - all is built twice
$(BINDIR)/.dummy:
	-@echo make directory $(BINDIR)
	-@mkdir $(BINDIR)
	-@touch $(BINDIR)/.dummy

# Clean up dependencies
# -----------------------------------------------------------

clean:
	-@rm ../bin/*/*.o
	-@rm ../bin/*/.dummy
	-@rm *~ ../doc/*~

clobber: clean
	-@rm -r ../bin/windows ../bin/osx ../bin/linux ../bin/generic
