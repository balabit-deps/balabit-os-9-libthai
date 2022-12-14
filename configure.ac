# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.71])

AC_INIT([libthai],
        [m4_esyscmd(build-aux/git-version-gen)],
        [https://github.com/tlwg/libthai/issues])
AC_CONFIG_SRCDIR([include/thai/thailib.h])
AC_CONFIG_AUX_DIR(build-aux)
AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE(dist-xz no-dist-gzip)

# Library versioning
# Library code modified:                    REVISION++
# Interfaces added:              CURRENT++  REVISION=0  AGE++
# Interfaces changed/removed:    CURRENT++  REVISION=0  AGE=0
LT_CURRENT=3
LT_REVISION=1
LT_AGE=3
AC_SUBST(LT_CURRENT)
AC_SUBST(LT_REVISION)
AC_SUBST(LT_AGE)

DOXYGEN_REQ_VER=1.9.1

dnl Checks for programs.
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
LT_INIT

# Whether linker support --version-script option
echo '{global:hello; local:*;};' > conftest.ver
_LT_LINKER_OPTION(
  [whether linker supports -version-script],
  [libthai_cv_have_version_script],
  [-Wl,-version-script -Wl,conftest.ver])

AM_CONDITIONAL(LD_HAS_VERSION_SCRIPT,
               test "$libthai_cv_have_version_script" = "yes")

dnl Check commandline options
AC_ARG_ENABLE(debug,
              [AS_HELP_STRING([--enable-debug],
                              [enable assertion checks])],
              , enable_debug="no")
AC_ARG_ENABLE(ansi,
              [AS_HELP_STRING([--enable-ansi],
                              [turn on strict ansi])],
              , enable_ansi="no")

if test "x$GCC" = "xyes"; then
  case " $CFLAGS " in
  *[[\ \	]]-Wall[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wall" ;;
  esac

  if test "x$enable_ansi" = "xyes"; then
    case " $CFLAGS " in
    *[[\ \	]]-ansi[[\ \	]]*) ;;
    *) CFLAGS="$CFLAGS -ansi" ;;
    esac

    case " $CFLAGS " in
    *[[\ \	]]-pedantic[[\ \	]]*) ;;
    *) CFLAGS="$CFLAGS -pedantic" ;;
    esac
  fi
fi
if test "x$enable_debug" = "xno"; then
  case " $CFLAGS " in
  *[[\ \	]]-DNDEBUG[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -DNDEBUG" ;;
  esac
fi

dnl Disable doc generation with doxygen option
AC_ARG_ENABLE(doxygen-doc,
              [AS_HELP_STRING([--disable-doxygen-doc],
                              [disable document generation with doxygen])],
              , enable_doxygen_doc="yes")

if test "x$enable_doxygen_doc" = "xyes"; then
  AC_CHECK_PROG(DOXYGEN,doxygen,doxygen,no)
  if test "x$DOXYGEN" = "xno"; then
    enable_doxygen_doc="no"
  else
    AC_MSG_CHECKING([doxygen >= $DOXYGEN_REQ_VER])
    DOXYGEN_VER=$($DOXYGEN --version)
    AX_COMPARE_VERSION([$DOXYGEN_VER],[ge],[DOXYGEN_REQ_VER], 
                       [AC_MSG_RESULT([$DOXYGEN_VER, yes])], 
                       [AC_MSG_RESULT([$DOXYGEN_VER, no, documentation disabled]); enable_doxygen_doc="no"])
  fi
fi

dnl where to install the doxygen-generated HTML doc
AC_ARG_WITH(html-docdir,
            [AS_HELP_STRING([--with-html-docdir=DIR],
                            [where to install the doxyten-generated HTML doc [PREFIX/share/doc/libthai/html]])],
            [htmldocdir="$withval"], [htmldocdir=\$\{prefix\}/share/doc/libthai/html])
AC_SUBST(htmldocdir)

AM_CONDITIONAL(ENABLE_DOXYGEN_DOC,test "x$enable_doxygen_doc" = "xyes")

dnl dictionary data generation
AC_ARG_ENABLE(dict,
              [AS_HELP_STRING([--disable-dict],
                              [disable dictionary data generation])],
              , enable_dict="yes")

if test "x$enable_dict" = "xyes"; then
  AC_CHECK_PROGS(TRIETOOL,[trietool-0.2 trietool],no)
  if test "x$TRIETOOL" = "xno"; then
    AC_MSG_ERROR([You need trietool[[-0.2]] (from libdatrie package) to generate dict, or just use --disable-dict to skip])
  fi
fi

AM_CONDITIONAL(ENABLE_DICT,test "x$enable_dict" = "xyes")

dnl Checks for libraries.
PKG_CHECK_MODULES(DATRIE,datrie-0.2)

dnl Checks for header files.
AC_CHECK_HEADERS([stddef.h stdlib.h string.h wchar.h])

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_SIZE_T

dnl Checks for library functions.
dnl AC_FUNC_MALLOC

AC_CONFIG_FILES([libthai.pc
  Makefile
  include/Makefile
  include/thai/Makefile
  src/Makefile
  src/thctype/Makefile
  src/thstr/Makefile
  src/thcell/Makefile
  src/thinp/Makefile
  src/thrend/Makefile
  src/thcoll/Makefile
  src/thbrk/Makefile
  src/thwchar/Makefile
  src/thwctype/Makefile
  src/thwstr/Makefile
  src/thwbrk/Makefile
  data/Makefile
  tests/Makefile
  doc/Makefile
  doc/Doxyfile
])
AC_OUTPUT

AC_MSG_RESULT([Type make to build libthai.])

