# s3backer on  Crossmeta FUSE Windows
s3backer on Crossmeta FUSE is port of @archiecobbs/3backer.  The Crossmeta system provides the POSIX environment and FUSE interface.  For building it uses the mingw32 cross compile environment on Linux.
## Porting Changes
Since the mingw32 tool chain is not aware of Crossmeta FUSE package, we have to manually configure the automake process.
* Edit configure.ac and change AC_MSG_ERROR to AC_MSG_WARN for FUSE pkg-config 
* Remove check for err.h sys/queue.h syslog.h unistd.h as they are not provided by Mingw32
* Set FUSE_CFLAGS environment variable to point to the Crossmeta FUSE header files

```export FUSE_CFLAGS='-DHAVE_CONFIG_H -D_DWIN32_LEAN_AND_MEAN -D_MSVCRT_ -D_POSIX -D_ALL_SOURCE -D _STAT_DEFINED -DNO_OLDNAMES -D_REENTRANT -D_INC_FCNTL -D_INC_STDLIB -D_IO_H_ -D_POSIX_C_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE64_SOURCE -DHAVE_LIBFUSE -D_POSIX_HOST_NAME_MAX=255 -I /home/user/cxfuse/include -I /home/user/cxfuse -I /home/suprasam/cxfuse/sys'```
* Set FUSE_LIBS environment variable

```export FUSE_LIBS='-lz -lexpat  -lcurl -lssh2 -lssl -lcrypto -lwldap32 -lidn -liconv /home/suprasam/cxfuse/lib/w2k/free/i386/libfuse.lib /home/user/cxfuse/lib/w2k/free/i386/cxfuse.dll /home/user/cxfuse/lib/w2k/free/i386/libfs.lib /home/user/cxfuse/lib/w2k/free/i386/libgen.lib -lgdi32 -lntdll -lws2_32'```

* ```./autogen.sh```
* ```/configure --host=i686-w64-mingw32```
* Edit Makefile and add $(FUSE_LIBS) in LIBS
```LIBS = -lz -lexpat -lcrypto -lcurl $(FUSE_LIBS)
* Edit s3backer.h and change syslog.h to sys/syslog.h
* Change all C99 standard %j to %I64 that MSVCRT supports.
