
m4_define([udisks_major_version], [2])
m4_define([udisks_minor_version], [1])
m4_define([udisks_micro_version], [7])

m4_define([udisks_version], [udisks_major_version.udisks_minor_version.udisks_micro_version])

AC_INIT([udisks],[udisks_version],[http://bugs.freedesktop.org/enter_bug.cgi?product=udisks],[udisks])

UDISKS_MAJOR_VERSION=udisks_major_version
UDISKS_MINOR_VERSION=udisks_minor_version
UDISKS_MICRO_VERSION=udisks_micro_version
UDISKS_VERSION=udisks_version
AC_SUBST(UDISKS_MAJOR_VERSION)
AC_SUBST(UDISKS_MINOR_VERSION)
AC_SUBST(UDISKS_MICRO_VERSION)
AC_SUBST(UDISKS_VERSION)

AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_AUX_DIR([build-aux])

AM_INIT_AUTOMAKE([1.11 foreign silent-rules dist-bzip2 no-dist-gzip])
AM_MAINTAINER_MODE
AM_SILENT_RULES([yes])

AX_CHECK_ENABLE_DEBUG

AC_PROG_CC
AC_ISC_POSIX
AC_HEADER_STDC
AC_PROG_LIBTOOL


# Compilation
#

GNOME_COMPILE_WARNINGS([maximum])

CC_CHECK_CFLAGS_APPEND([                          \
        -Waggregate-return                        \
        -Wall                                     \
        -W                                        \
        -Wextra                                   \
        -Wcast-align                              \
        -Wdeclaration-after-statement             \
        -Wendif-labels                            \
        -Werror=format-security                   \
        -Werror=implicit-function-declaration     \
        -Werror=overflow                          \
        -Wformat-nonliteral                       \
        -Wformat=2                                \
        -Wimplicit                                \
        -Winit-self                               \
        -Wlogical-op                              \
        -Wmissing-declarations                    \
        -Wmissing-format-attribute                \
        -Wmissing-include-dirs                    \
        -Wmissing-noreturn                        \
        -Wmissing-prototypes                      \
        -Wnested-externs                          \
        -Wno-inline                               \
        -Wno-long-long                            \
        -Wno-missing-field-initializers           \
        -Wno-overlength-strings                   \
        -Wno-switch-enum                          \
        -Wno-unused-parameter                     \
        -Wno-unused-result                        \
        -Wold-style-definition                    \
        -Wpacked                                  \
        -Wpointer-arith                           \
        -Wshadow                                  \
        -Wsign-compare                            \
        -Wstrict-aliasing=2                       \
        -Wstrict-prototypes                       \
        -Wundef                                   \
        -Wuninitialized                           \
        -Wmaybe-uninitialized                     \
        -Wunsafe-loop-optimizations               \
        -Wvla                                     \
        -Wwrite-strings                           \
        -fdata-sections                           \
        -fdiagnostics-show-option                 \
        -ffast-math                               \
        -ffunction-sections                       \
        -fno-common                               \
        -fno-strict-aliasing                      \
        -D_LARGEFILE64_SOURCE                     \
        -D_FILE_OFFSET_BITS=64                    \
])

AC_PATH_PROG([XSLTPROC], [xsltproc])
if test -z "$XSLTPROC"; then
  AC_MSG_ERROR([xsltproc is needed])
fi

GTK_DOC_CHECK([1.3],[--flavour no-tmpl])

AC_ARG_ENABLE(man,
              [AS_HELP_STRING([--enable-man],
                              [generate man pages [default=auto]])],,
              enable_man=maybe)
AS_IF([test "$enable_man" != no], [
  AC_PATH_PROG([XSLTPROC], [xsltproc])
  AS_IF([test -z "$XSLTPROC"], [
    AS_IF([test "$enable_man" = yes], [
      AC_MSG_ERROR([xsltproc is required for --enable-man])
    ])
    enable_man=no
  ])
  enable_man=yes
])
AM_CONDITIONAL(ENABLE_MAN, test "$enable_man" != no)

GOBJECT_INTROSPECTION_CHECK([0.6.2])

# Behavior
#

AC_ARG_ENABLE(fhs-media,
              [AS_HELP_STRING([--enable-fhs-media],
                              [Mount devices in /media instead of /run/media [default=no]])],
              fhs_media=yes,
              fhs_media=no)
if test "x$fhs_media" = "xyes"; then
  AC_DEFINE([HAVE_FHS_MEDIA], 1, [Define to 1 to use /media for mounting])
fi

# Libraries
#

PKG_CHECK_MODULES(GUDEV, [gudev-1.0 >= 165])
AC_SUBST(GUDEV_CFLAGS)
AC_SUBST(GUDEV_LIBS)

PKG_CHECK_MODULES(GLIB, [glib-2.0 >= 2.36])
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

PKG_CHECK_MODULES(GIO, [gio-unix-2.0 >= 2.36])
AC_SUBST(GIO_CFLAGS)
AC_SUBST(GIO_LIBS)

PKG_CHECK_MODULES(POLKIT_GOBJECT_1, [polkit-gobject-1 >= 0.102])
AC_SUBST(POLKIT_GOBJECT_1_CFLAGS)
AC_SUBST(POLKIT_GOBJECT_1_LIBS)

PKG_CHECK_MODULES(POLKIT_AGENT_1, [polkit-agent-1 >= 0.102])
AC_SUBST(POLKIT_AGENT_1_CFLAGS)
AC_SUBST(POLKIT_AGENT_1_LIBS)

PKG_CHECK_MODULES(LIBATASMART, [libatasmart >= 0.17])
AC_SUBST(LIBATASMART_CFLAGS)
AC_SUBST(LIBATASMART_LIBS)

PKG_CHECK_MODULES(LIBSYSTEMD_LOGIN, [libsystemd >= 209], [have_libsystemd_login=yes],
                  [PKG_CHECK_MODULES(LIBSYSTEMD_LOGIN, [libsystemd-login >= 44 libsystemd-daemon],
                  [have_libsystemd_login=yes],
                  [have_libsystemd_login=no])])
AM_CONDITIONAL(HAVE_LIBSYSTEMD_LOGIN, test x$have_libsystemd_login = xyes)
if test "x$have_libsystemd_login" = "xyes"; then
  AC_DEFINE([HAVE_LIBSYSTEMD_LOGIN], 1, [Define to 1 if libsystemd-login is available])
fi
AC_SUBST(HAVE_LIBSYSTEMD_LOGIN)
AC_SUBST(LIBSYSTEMD_LOGIN_CFLAGS)
AC_SUBST(LIBSYSTEMD_LOGIN_LIBS)

# udevdir
AC_ARG_WITH([udevdir],
            AS_HELP_STRING([--with-udevdir=DIR], [Directory for udev]),
            [],
            [with_udevdir=$($PKG_CONFIG --variable=udevdir udev)])
AC_SUBST([udevdir], [$with_udevdir])

# systemd
AC_ARG_WITH([systemdsystemunitdir],
            AS_HELP_STRING([--with-systemdsystemunitdir=DIR], [Directory for systemd service files]),
            [],
            [with_systemdsystemunitdir=$($PKG_CONFIG --variable=systemdsystemunitdir systemd)])
if test "x$with_systemdsystemunitdir" != "xno"; then
  AC_SUBST([systemdsystemunitdir], [$with_systemdsystemunitdir])
fi
AM_CONDITIONAL(HAVE_SYSTEMD, [test -n "$systemdsystemunitdir"])

have_acl=no
AC_ARG_ENABLE(acl, AS_HELP_STRING([--disable-acl], [disable acl support]))
if test "x$enable_acl" != "xno"; then
  AC_CHECK_HEADERS(
          [sys/acl.h acl/libacl.h],
          [
            AC_CHECK_LIB(
                [acl],
                [acl_get_file],
                [AC_DEFINE(HAVE_ACL, 1, [Define if libacl is available]) have_acl=yes],
                have_acl=no)
          ],
          have_acl=no)
  if test "x$have_acl" = "xyes"; then
    ACL_CFLAGS=""
    ACL_LIBS="-lacl"
  fi
  AC_SUBST(ACL_CFLAGS)
  AC_SUBST(ACL_LIBS)
  if test "x$have_acl" = xno -a "x$enable_acl" = xyes; then
    AC_MSG_ERROR([acl support requested but libraries not found])
  fi
fi
AM_CONDITIONAL(HAVE_ACL, [test "$have_acl" = "yes"])


# Internationalization
#

IT_PROG_INTLTOOL([$INTLTOOL_REQUIRED])
GETTEXT_PACKAGE=udisks2
AC_SUBST([GETTEXT_PACKAGE])
AM_GLIB_GNU_GETTEXT
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE],["$GETTEXT_PACKAGE"],[gettext domain])

# Generate
#

AC_OUTPUT([
Makefile
data/Makefile
udisks/Makefile
udisks/udisks2.pc
src/Makefile
src/tests/Makefile
tools/Makefile
doc/Makefile
doc/version.xml
doc/man/Makefile
po/Makefile.in
])

dnl ==========================================================================
echo "
               udisks $VERSION
             ================

        prefix:                     ${prefix}
        libdir:                     ${libdir}
        libexecdir:                 ${libexecdir}
        bindir:                     ${bindir}
        sbindir:                    ${sbindir}
        datadir:                    ${datadir}
        sysconfdir:                 ${sysconfdir}
        localstatedir:              ${localstatedir}
        docdir:                     ${docdir}
        introspection:              ${found_introspection}

        udevdir:                    ${udevdir}
        systemdsystemunitdir:       ${systemdsystemunitdir}
        using libsystemd-login:     ${have_libsystemd_login}
        use /media for mounting:    ${fhs_media}
        acl support:                ${have_acl}

        compiler:                   ${CC}
        cflags:                     ${CFLAGS}
        cppflags:                   ${CPPFLAGS}

        Maintainer mode:            ${USE_MAINTAINER_MODE}
        Building api docs:          ${enable_gtk_doc}
        Building man pages:         ${enable_man}
"
