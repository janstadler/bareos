VPATH = .:../../filed:../generic

include ../Makefile.inc

DEFINES += -DWIN32_VSS

ifeq ($(WIN_VERSION),32)
INCLUDES = -I. \
	   -I../.. \
	   -I../../include \
	   -I../include \
	   -I../compat/include \
	   -I../vss/include
else
INCLUDES = -I. \
	   -I../.. \
	   -I../../include \
	   -I../include \
	   -I../compat/include \
	   -I../vss/include
DEFINES += -DHAVE_VSS64
endif
LDLIBS = ../lib/libbareos.a \
	 ../lib/libbareos.dll \
	 ../findlib/libbareosfind.dll \
         $(MINGW_LIB)/libz.dll.a \
         $(MINGW_LIB)/liblzo2.dll.a \
         $(MINGW_LIB)/libfastlz.dll.a \
         $(MINGW_LIB)/libpthreadGCE2.a \
	 $(WINSOCKLIB) -lole32 -loleaut32 -luuid -lcomctl32

SVRSRCS = accurate.c authenticate.c backup.c compression.c \
	  crypto.c dir_cmd.c estimate.c fd_plugins.c filed_conf.c \
	  filed.c fileset.c heartbeat.c restore.c sd_cmds.c status.c verify.c \
	  verify_vol.c vss.c vss_XP.c vss_W2K3.c vss_Vista.c \
	  service.c main.c

SVROBJS = $(SVRSRCS:.c=.o)

WINDRESSRCS = filedres.rc
WINDRESOBJS = $(WINDRESSRCS:.rc=.o)

all: bareos-fd.exe

bareos-fd.exe: DLL_USAGE = -DUSING_DLL
bareos-fd.exe: $(SVROBJS) $(WINDRESOBJS)
	$(CXX) $(LDFLAGS_WINAPP) $(SVROBJS) $(WINDRESOBJS) $(LDLIBS) -o $@

clean:
	rm -f *.o

distclean:: clean
	rm -f bareos-fd.exe

# inference rules
.c.o:
	@echo "Compiling $<"
	$(CXX) $(DLL_USAGE) $(CXXFLAGS) -c -o $@ $<

%.o : %.rc
	@echo "Ressource Compiling $^"
	$(WINDRES) $^ -o $@

main.o: ../generic/main.c
	@echo "Compiling $<"
	$(CXX) -DFILE_DAEMON $(DLL_USAGE) $(CXXFLAGS) -c -o $@ $<
