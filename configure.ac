# version of meanwhile
m4_define(meanwhile_major,	0)
m4_define(meanwhile_minor,	1)
m4_define(meanwhile_micro,	0)

# release of meanwhile. 0 for CVS, 1 for release
m4_define(meanwhile_release,	0)

# required and compat version of glib2.
m4_define(glib_required_version,	2.0.0)

AC_INIT([meanwhile tears],[meanwhile_major.meanwhile_minor.meanwhile_micro],[pg8p@virginia.edu])
AC_CONFIG_SRCDIR([src/common.c])
AM_INIT_AUTOMAKE([-Wall -Werror foreign])
AM_PROG_AR([ACT-IF-FAIL])

RELEASE=arsenic
AC_SUBST(RELEASE)

AC_PREREQ([2.69])

AM_MAINTAINER_MODE

AC_PROG_CC
AC_PROG_CXX
AC_PROG_AWK
AC_PROG_CC
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_RANLIB
LT_INIT
LIBTOOL="$LIBTOOL --silent"

# Use the C language and compiler for the following checks
AC_LANG([C])

AC_HEADER_STDC

# current:revision:age
MW_SO_VERSION=$((meanwhile_major+meanwhile_minor)):meanwhile_micro:meanwhile_minor
AC_SUBST(MW_SO_VERSION)



# os-specific so flags
case "${host}" in
*darwin)
	MW_SO_OS_FLAGS="-dynamic-lib";;
*mingw32 | *cygwin)
	MW_SO_OS_FLAGS="-shared -no-undefined";;
*)
	MW_SO_OS_FLAGS="-shared";;
esac
AC_SUBST(MW_SO_OS_FLAGS)



# Debugging option
AC_ARG_ENABLE(debug,
	[  --enable-debug	  compile with debugging support],
	AC_DEFINE(DEBUG, 1, [Define if debugging is enabled.]), )
if test "$enable_debug" = "yes" ; then
    CFLAGS="$CFLAGS  -g -O0 -Weverything -Wall -Wno-uninitialized -Wextra"
    CXXFLAGS="$CXXFLAGS -g -O0 -Weverything -Wall -Wno-uninitialized -Wextra"
fi


# debugging output urging people to send mail
enableval="yes"
AC_ARG_ENABLE(mailme,
	[  --enable-mailme[[=yes]]   enable mw_debug_mailme output], )

MW_MAILME=1
if test "$enableval" = "no" ; then
   MW_MAILME=0
fi
AC_SUBST(MW_MAILME)


# Enable gcov suport.
AC_ARG_ENABLE([gcov],
AS_HELP_STRING([--enable-gcov],[build binaries with gcov support]), [use_gcov=yes], [use_gcov=no])
if test "x$use_gcov" = xyes; then
  if test "x$CLANG" = xyes; then
    CFLAGS="$CFLAGS -fprofile-arcs -ftest-coverage"
  else
    CFLAGS="$CFLAGS --coverage"
  fi
fi
AM_CONDITIONAL(GCOV, test "$use_gcov" = yes)

if test "x$use_gcov" = xyes; then
  CFLAGS="$CFLAGS -O0 -g -Wall"
else
  CFLAGS="$CFLAGS $OLD_CFLAGS"
fi


# Doxygen generation option
enableval="yes"
AC_ARG_ENABLE(doxygen,
	[  --enable-doxygen[[=yes]]  enable the doc package], )

enable_doxygen=$enableval

AM_CONDITIONAL(ENABLE_DOXYGEN, test "$enable_doxygen" = "yes")



# doc sub package
# right now this depends only on doxygen
WITH_DOC_SUBPACKAGE=
if test "$enable_doxygen" = "yes" ; then
   WITH_DOC_SUBPACKAGE=1
fi
AC_SUBST(WITH_DOC_SUBPACKAGE)



# libm for use in mpi.c
AC_CHECK_LIB(m, main, LIBM="-lm")
AC_CHECK_HEADER(math.h)
AC_SUBST(LIBM)

AC_CHECK_HEADERS([limits.h netdb.h netinet/in.h stdlib.h string.h sys/socket.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T

# Checks for library functions.
AC_CHECK_FUNCS([gethostbyname memset socket strchr strrchr strstr])

# Glib-2.0
PKG_CHECK_MODULES(GLIB,
[glib-2.0 >= glib_required_version],
[
	AC_DEFINE(HAVE_GLIB, 1, [Define if we've found glib.])
])

GLIB_VERSION=glib_required_version
AC_SUBST(GLIB_VERSION)
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

AC_CONFIG_MACRO_DIR([m4])

AC_CONFIG_FILES(
	[Makefile src/Makefile src/mpi/Makefile]
	[samples/Makefile]
	[doc/Makefile doc/Doxyfile]
	[meanwhile.spec meanwhile.pc]
)

AC_CONFIG_FILES([samples/build], [chmod +x samples/build])

AC_CONFIG_HEADERS([config.h])

AC_OUTPUT



echo

echo -n "mailme debug mode........ : "
if test "$MW_MAILME" = 1 ; then
   echo "enabled"
else
   echo "disabled"
fi

echo -n "Doxygen generation....... : "
if test "$enable_doxygen" = "yes" ; then
   echo "enabled"
else
   echo "disabled"
fi

echo
echo configure complete, now run \`make\`
echo
echo you may need to run \`ldconfig\` as root after installation before
echo being able to load this library
echo

#: The End.
