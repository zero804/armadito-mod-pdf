
AC_INIT(armadito-pdf, [0.12.6], ufausther@teclib.com)
AM_INIT_AUTOMAKE([foreign])

# collect arguments
# debug
AC_ARG_ENABLE([debug],
	AS_HELP_STRING([--enable-debug],[enable debugging @<:@default is yes@:>@]),
	,
	enable_debug="yes")

# armadito pdf library
AC_ARG_ENABLE([lib],
	AS_HELP_STRING([--enable-lib],[enable armadito-pdf library @<:@default is no@:>@]),
	enable_lib="yes",
	enable_lib="no")

# module
AC_ARG_ENABLE([module],
	AS_HELP_STRING([--enable-module],[enable armadito-av module @<:@default is no@:>@]),
	enable_module="yes",
	enable_module="no")


# cli tools
AC_ARG_ENABLE([tools],
	AS_HELP_STRING([--enable-tools],[enable armadito-pdf cli tools @<:@default is no@:>@]),
	[enable_tools="yes"],
	[enable_tools="no"])

PKG_PROG_PKG_CONFIG

# check for analysis module sources
AC_MSG_CHECKING(for armadito-pdf library sources)
AC_CONFIG_SRCDIR(lib/src/armaditopdf.c)
AC_MSG_RESULT(yes)

# check for needed programs
AC_PROG_CC([gcc])
AC_PROG_LIBTOOL

# check for debug arg
AC_MSG_CHECKING(for debug)
AC_MSG_RESULT($enable_debug)
AM_CONDITIONAL([DEBUG], [test "$enable_debug" = "yes"])
if test "$enable_debug" = "yes"; then
   AC_DEFINE(DEBUG,1,[Define DEBUG to enable debug messages])
   CFLAGS="${CFLAGS} -g"
fi


# check for library arg
AC_MSG_CHECKING(for armadito PDF library)
AC_MSG_RESULT($enable_lib)
AM_CONDITIONAL([COND_LIBRARY], [test "$enable_lib" = "yes"])

# check for module arg
AC_MSG_CHECKING(for armadito module)
AC_MSG_RESULT($enable_module)
AM_CONDITIONAL([COND_MODULE], [test "$enable_module" = "yes"])

# check for libarmadito if module is enabled.
if test "$enable_module" = "yes"; then
	# check for libarmadito
	PKG_CHECK_MODULES(LIBARMADITO, libarmadito, [HAVE_LIBARMADITO=yes], [HAVE_LIBARMADITO=no])
	# check for libarmadito/armadito.h in source tree
	if test "$HAVE_LIBARMADITO" = "no"; then
	   OLD_CPPFLAGS="$CPPFLAGS"
	   CPPFLAGS=-I$srcdir/../../armadito-core/libmodule/include
	   AC_CHECK_HEADER([libarmadito/armadito.h], [], [AC_MSG_ERROR([cannot find Armadito module library])], [])
	   CPPFLAGS="$OLD_CPPFLAGS"
	   LIBARMADITO_CFLAGS='-I$(top_srcdir)/../../armadito-core/libmodule/include'
	   AC_SUBST([LIBARMADITO_CFLAGS])
	fi
fi

# check for tools arg
AC_MSG_CHECKING(for cli tools)
AC_MSG_RESULT($enable_tools)
AM_CONDITIONAL([COND_TOOLS], [test "$enable_tools" = "yes"])

# check for libarmadito-pdf if module is enabled.
if test "$enable_tools" = "yes"; then

	# check for libarmadito-pdf
	PKG_CHECK_MODULES(LIBARMADITO_PDF, libarmadito-pdf, [HAVE_LIBARMADITO_PDF=yes], [HAVE_LIBARMADITO_PDF=no])
	if test "$HAVE_LIBARMADITO_PDF" = "no"; then
		AC_MSG_ERROR("cannot find ARMADITO-PDF library: skipping armadito-pdf tool build.")
	fi
fi


# Output files
AC_CONFIG_FILES([
Makefile
])

# conditional makefile for library.
AM_COND_IF([COND_LIBRARY],
	AC_CONFIG_FILES([
	lib/Makefile
]))

# conditional makefile for tools.
AM_COND_IF([COND_TOOLS],
	AC_CONFIG_FILES([
	tools/Makefile
	tools/armadito-pdf/Makefile
]))

# conditional makefile for module.
AM_COND_IF([COND_MODULE],
	AC_CONFIG_FILES([
	module/Makefile
]))

AC_OUTPUT
