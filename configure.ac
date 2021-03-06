dnl#                                               -*- Autoconf -*-
dnl# Process this file with autoconf to produce a configure script.
AC_PREREQ([2.68])
AC_INIT([stress],[1.0.4],[apw@rossby.metr.ou.edu])
AC_CONFIG_SRCDIR([src/stress.c])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])
if test ! -e ${srcdir}/build-aux/install-sh; then
  AC_MSG_NOTICE([install-sh is not present; autoreconf-ing for you])
  AC_PATH_PROGS([AUTORECONF],
                [autoreconf autoreconf264 autoreconf263 autoreconf213],
                [AC_MSG_ERROR([autoreconfing is needed but impossible])],
                [${PATH}:/opt/local/bin:/sw/bin:/sw/lib/autoconf2.13/bin])
  cd ${srcdir} && ${AUTORECONF} -fvi -Wall
  AC_MSG_NOTICE([done. Exiting so you can try to configure again.])
  exit 1
else
  test ! -z "${AUTORECONF}" && export AUTORECONF
  AC_SUBST([AUTORECONF])
fi

AC_CONFIG_AUX_DIR([build-aux])
AC_CANONICAL_TARGET dnl# also calls the "_HOST" version
dnl# need a line of whitespace:

dnl# now that we have checked that, we can check automake:
if test "x${am__api_version}" = "x"; then
  test -z "${am__api_version}" && export am__api_version='1.11'
fi
AM_INIT_AUTOMAKE([1.11 gnits])
export ACLOCAL=aclocal
export AUTOMAKE=automake
AM_MAINTAINER_MODE
AM_SANITY_CHECK
dnl# AX_* macros come from the autoconf-archive package:
AX_SPEC_PACKAGE_AND_VERSION([Makefile.spec])dnl

dnl# Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_GCC_TRADITIONAL
PKG_PROG_PKG_CONFIG
AC_PATH_PROG([HELP2MAN],[help2man],[:],[${PATH}:/opt/local/bin:/sw/bin])
AM_MISSING_PROG([HELP2MAN],[help2man])
AC_SUBST([HELP2MAN])dnl
AC_PATH_PROG([MAKEINFO],[makeinfo],[],[/sw/opt/texinfo-legacy/bin:${PATH}])
AM_MISSING_PROG([MAKEINFO],[makeinfo])
AC_SUBST([MAKEINFO])dnl
AC_PATH_PROGS([AUTOSCAN],[autoscan autoscan264 autoscan263 autoscan213],
              [touch configure.scan],
              [${PATH}:/opt/local/bin:/sw/bin:/sw/lib/autoconf2.13/bin])
AC_SUBST([AUTOSCAN])dnl
LT_INIT([disable-static])
LTVERSION_VERSION

# keep autoscan happy:
if test "x0" = "Y1"; then
  test -x "`which autoscan`"
  AC_PROG_RANLIB
fi

dnl# Checks for libraries.
LT_LIB_M
AC_CHECK_LIB([m],[sqrt])
AC_CHECK_LIB([gen],[basename])dnl

dnl# Checks for header files.
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS([ctype.h errno.h libgen.h math.h stdio.h signal.h time.h])dnl

dnl# Checks for typedefs, structures, and compiler characteristics.
AC_SYS_LARGEFILE
AC_SUBST([ac_cv_sys_largefile_CC])dnl
AC_SUBST([ac_cv_sys_file_offset_bits])dnl
AC_SUBST([ac_cv_sys_large_files])dnl
AC_C_BIGENDIAN
AC_C_CONST
AC_C_PROTOTYPES
AC_CHECK_TYPES([long long])dnl

dnl# Checks for library functions.
AC_FUNC_FORK
AC_FUNC_MALLOC
AC_CHECK_FUNCS([alarm close exit fflush fprintf free getpid kill mkstemp \
                perror rand signal sleep sqrt sscanf strcmp strerror sync \
                time unlink usleep wait write])dnl

dnl# Options that alter compile and link.
dnl# change name to avoid conflict with the libtool-provided one:
AC_ARG_ENABLE([staticlib],
               [AS_HELP_STRING([--enable-staticlib],
                               [build static library @<:@default=no@:>@])],
               [staticlib=${enableval}],
               [staticlib=no])dnl

dnl# Warning: this can make "`make distcheck`" fail:
if test "x${staticlib}" = "xyes"; then
    # if we are using gcc, add `-static' to LDFLAGS
    if test -n "${GCC}" || test "x${ac_cv_c_compiler_gnu}" = "xyes"; then
        export STATIC_LD="-static"
        export LDFLAGS="${LDFLAGS} -static"
        AC_SUBST([STATIC_LD])
        AC_SUBST([LDFLAGS])
    fi
fi

AM_CONDITIONAL([BUILD_STATICLIB],[test "x${STATIC_LD}" = "x-static"])dnl

# Output.
AC_CONFIG_FILES([Makefile \
src/Makefile \
doc/Makefile \
test/Makefile \
build-aux/Makefile \
m4/Makefile])dnl

AC_OUTPUT
