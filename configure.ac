dnl Process this file with autoconf to produce a configure script.
AC_INIT(Tools/CmdLine/IccDumpProfile/iccDumpProfile.cpp)

dnl Checks for target
AC_CANONICAL_TARGET

SAMPLEICC_MAJOR_VERSION=1
SAMPLEICC_MINOR_VERSION=6
SAMPLEICC_MICRO_VERSION=7

SAMPLEICC_VERSION=$SAMPLEICC_MAJOR_VERSION.$SAMPLEICC_MINOR_VERSION.${SAMPLEICC_MICRO_VERSION}
AC_SUBST(SAMPLEICC_VERSION)

LIBTOOL_CURRENT=2
LIBTOOL_REVISION=3
LIBTOOL_AGE=0
LIBTOOL_VERSION=$LIBTOOL_CURRENT:$LIBTOOL_REVISION:$LIBTOOL_AGE
AC_SUBST(LIBTOOL_VERSION)

dnl GNU automake support.
AM_INIT_AUTOMAKE(SampleICC, ${SAMPLEICC_VERSION})
AM_MAINTAINER_MODE

dnl Checks for programs.
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_CC
AC_PROG_LN_S
AM_PROG_LIBTOOL
AC_PROG_MAKE_SET

dnl Checks for libraries.

dnl Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS(limits.h unistd.h)

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_C_INLINE
AC_TYPE_SIZE_T

dnl Checks for library functions.
AC_CHECK_FUNCS(strerror)

dnl Checks for libtiff.
dnl Basically a cut-and-paste of ab_lib_id3.m4 from Oskar Liljeblad
dnl (oskar@osk.mine.nu) as published in the autoconf archive, downloadable
dnl at http://autoconf-archive.cryp.to/ac_lib_id3.m4.
dnl
dnl This macro will check for the existence of libtiff
dnl (http://www.libtiff.org/). It does this by checking for the
dnl header file tiff.h and the TIFF library object file. A --with-tifflib
dnl option is supported as well. The following output variables are set
dnl with AC_SUBST:
dnl
dnl   TIFF_CPPFLAGS
dnl   TIFF_LDFLAGS
dnl   TIFF_LIBS
dnl
dnl You can use them like this in Makefile.am:
dnl
dnl   AM_CPPFLAGS = $(TIFF_CPPFLAGS)
dnl   AM_LDFLAGS = $(TIFF_LDFLAGS)
dnl   program_LDADD = $(TIFF_LIBS)
dnl
dnl Additionally, the C preprocessor symbol HAVE_TIFFLIB will be defined
dnl with AC_DEFINE if libtiff is available.

AC_DEFUN([AC_LIB_TIFF], [
  AH_TEMPLATE([HAVE_TIFFLIB], [Define if libtiff is available])
  AC_ARG_WITH(tifflib, [  --with-tifflib=DIR       prefix for TIFF library files and headers], [
    if test x"$withval" = "xno"; then
      ac_tiff_path=
      $2
    elif test x"$withval" = x"yes"; then
      ac_tiff_path=/opt/local
    else
      ac_tiff_path="$withval"
    fi
  ],[ac_tiff_path=/opt/local])
  if test x"$ac_tiff_path" != "x"; then
    saved_CPPFLAGS="$CPPFLAGS"
    CPPFLAGS="$CPPFLAGS -I$ac_tiff_path/include"
    AC_CHECK_HEADER([tiff.h], [
      saved_LDFLAGS="$LDFLAGS"
      LDFLAGS="$LDFLAGS -L$ac_tiff_path/lib"
      AC_CHECK_LIB(tiff, TIFFOpen, [
        AC_SUBST(TIFF_CPPFLAGS, [-I$ac_tiff_path/include])
        AC_SUBST(TIFF_LDFLAGS, [-L$ac_tiff_path/lib])
        AC_SUBST(TIFF_LIBS, [-ltiff])
        AC_SUBST(SICC_ICC_APPLY_PROFILES, [IccApplyProfiles])
	AC_DEFINE([HAVE_TIFFLIB])
        $1
      ], [AC_MSG_WARN([
*** SampleICC requires a recent version of libtiff (3.6.1 or newer), which
*** you don't appear to have, if it is to create the iccApplyProfiles
*** command-line executable.
***
*** Interestingly, you have a tiff.h header, but the run-time linker is not
*** finding libtiff, or it is finding the wrong version, or there is some other
*** problem trying a test link against libtiff.
***
*** If you believe you have the TIFF headers and libraries, but they are not
*** in the standard locations (e.g. /usr/include or /opt/local/include) then
*** supply the root of this hierarchy to configure with --with-tifflib.  For
*** example if you keep the headers in /usr/people/fred/include and the libraries
*** in /usr/people/fred/lib, then specify --with-tifflib=/usr/people/fred.
***
*** On some systems, you'll need
*** to set your LD_LIBRARY_PATH environment variable, or edit /etc/ld.so.conf
*** to point to the proper version.  Also, make sure you have run ldconfig if
*** that is required on your system.
***
*** In the meantime, the generated hierarchy of Makefiles will not attempt to
*** build an iccApplyProfiles command-line executable, but everything else
*** (the SampleICC library and the other command-line executables) will be built.
			   ])
          AC_SUBST(SICC_ICC_APPLY_PROFILES, [])],
	-lm)
      LDFLAGS="$saved_LDFLAGS"
    ], [
      AC_MSG_WARN([
*** SampleICC requires the header files and libraries for a recent version of
*** the TIFF libraries (libtiff 3.6.1 or newer), if it is to create the
*** iccApplyProfiles command-line executable after it compiles the SampleICC
*** library and the other command-line executables.
***
*** You don't have the expected tiff.h header, so the link-time check for
*** a static or dynamic TIFF library was skipped.
***
*** If you believe you have the TIFF headers and libraries, but they are not
*** in the standard locations (e.g. /usr/include or /opt/local/include) then
*** supply the root of this hierarchy to configure with --with-tifflib.  For
*** example if you keep the headers in /usr/people/fred/include and the libraries
*** in /usr/people/fred/lib, then specify --with-tifflib=/usr/people/fred.
***
*** In the meantime, the generated hierarchy of Makefiles will not attempt to
*** build an iccApplyProfiles executable, but everything else will be built.
			   ])
      AC_SUBST(SICC_ICC_APPLY_PROFILES, [])
    ])
    CPPFLAGS="$saved_CPPFLAGS"
  fi
])

AC_LIB_TIFF

dnl Platform-specific stuff
case "$host" in
*irix*) 
  AC_DEFINE(PLATFORM_IRIX) 
  ;;
*solaris*) 
  AC_DEFINE(PLATFORM_SUNOS5) 
  ;;
*osf*) 
  AC_DEFINE(PLATFORM_OSF1) 
  ;;
*darwin*) 
  AC_DEFINE(PLATFORM_DARWIN_PPC) 
  OSX_APPLICATION_LIBS="-framework ApplicationServices"
  ;;
esac

dnl Processor-specific optimizations
AC_MSG_CHECKING(for processor-specific optimizations)
case $target_cpu in
i686) EXTRA_CPU_CFLAGS=$(if $CC -march=i686 -S -o /dev/null -xc /dev/null > /dev/null; then echo "-march=i686"; fi) ;;
*athlon*)  EXTRA_CPU_CFLAGS=$(if $CC -march=i686 -S -o /dev/null -xc /dev/null > /dev/null; then echo "-march=i686"; fi) ;;
esac
AC_MSG_RESULT([$EXTRA_CPU_CFLAGS])

AM_CFLAGS="$EXTRA_OPT_CFLAGS $EXTRA_CPU_CFLAGS"
AM_CXXFLAGS="$EXTRA_OPT_CFLAGS $EXTRA_CPU_CFLAGS"

AC_SUBST(AM_CFLAGS)
AC_SUBST(AM_CXXFLAGS)
AC_SUBST(OSX_APPLICATION_LIBS)
AC_SUBST(SAMPLEICC_MAJOR_VERSION)
AC_SUBST(SAMPLEICC_MINOR_VERSION)
AC_SUBST(SAMPLEICC_MICRO_VERSION)

AC_OUTPUT([
Makefile
sampleicc.pc
IccProfLib/Makefile
Tools/Makefile
Tools/CmdLine/Makefile
Tools/CmdLine/IccApplyNamedCmm/Makefile
Tools/CmdLine/IccApplyProfiles/Makefile
Tools/CmdLine/IccDumpProfile/Makefile
Tools/CmdLine/IccGamutMapGirdle/Makefile
Tools/CmdLine/IccGetBPCInfo/Makefile
Tools/CmdLine/IccProfLibTest/Makefile
Tools/CmdLine/IccRoundTrip/Makefile
Tools/CmdLine/IccStripUnknownTags/Makefile
Tools/CmdLine/IccV4ToMPE/Makefile
Contrib/Makefile
Contrib/CmdLine/Makefile
Contrib/CmdLine/create_CLUT_profile/Makefile
Contrib/CmdLine/create_CLUT_profile_from_probe/Makefile
Contrib/CmdLine/create_display_profile/Makefile
Contrib/ICC_utils/Makefile
Contrib/Mac_OS_X/Makefile
Contrib/Mac_OS_X/create_probe/Makefile
Contrib/Mac_OS_X/extract_probe_data/Makefile
Contrib/examples/Makefile
Contrib/examples/RSR/Makefile
Contrib/tests/Makefile
Contrib/tests/flatten_AToB_tag/Makefile
Contrib/tests/generate_device_codes/Makefile
Contrib/tests/reconstruct_measurements/Makefile
Contrib/tests/round_trip_PCS_LAB/Makefile
])
