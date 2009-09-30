# vi:set et ai sw=2 sts=2 ts=2: */
# -
# Copyright (c) 2009 Jannis Pohlmann <jannis@xfce.org>
# 
# This program is free software; you can redistribute it and/or 
# modify it under the terms of the GNU General Public License as
# published by the Free Software Foundation; either version 2 of 
# the License, or (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the 
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public 
# License along with this program; if not, write to the Free 
# Software Foundation, Inc., 51 Franklin Street, Fifth Floor,
# Boston, MA 02110-1301, USA.

dnl ***************************
dnl *** Version information ***
dnl ***************************
m4_define([tumbler_verinfo], [0:0:0]) dnl TODO: Do we need to change this for the first release?
m4_define([tumbler_version_api_major], [1])
m4_define([tumbler_version_major], [0])
m4_define([tumbler_version_minor], [0])
m4_define([tumbler_version_micro], [0])
m4_define([tumbler_version_build], [r@REVISION@])
m4_define([tumbler_version_tag], [svn])
m4_define([tumbler_version], [tumbler_version_major().tumbler_version_minor().tumbler_version_micro()ifelse(tumbler_version_tag(), [svn], [tumbler_version_tag()-tumbler_version_build()], [tumbler_version_tag()])])

dnl ********************************************
dnl *** Full debug support for SVN snapshots ***
dnl ********************************************
m4_define([tumbler_debug_default], [ifelse(tumbler_version_tag(), [svn], [full], [minimum])])

dnl ***************************
dnl *** Initialize autoconf ***
dnl ***************************
AC_COPYRIGHT([Copyright (c) 2009 Jannis Pohlmann <jannis@xfce.org>])
AC_INIT([tumbler], [tumbler_version], [http://bugzilla.xfce.org/])
AC_PREREQ([2.50])
AC_REVISION([@REVISION@])
AC_CANONICAL_TARGET()

dnl ***************************
dnl *** Initialize automake ***
dnl ***************************
AM_INIT_AUTOMAKE([AC_PACKAGE_TARNAME()], [AC_PACKAGE_VERSION()])
AM_CONFIG_HEADER([config.h])
AM_MAINTAINER_MODE()

dnl **************************
dnl *** Libtool versioning ***
dnl **************************
TUMBLER_VERINFO=tumbler_verinfo()
AC_SUBST([TUMBLER_VERINFO])

dnl ****************************
dnl *** Subst tumbler version ***
dnl ****************************
TUMBLER_VERSION_MAJOR=tumbler_version_major()
TUMBLER_VERSION_MINOR=tumbler_version_minor()
TUMBLER_VERSION_MICRO=tumbler_version_micro()
TUMBLER_VERSION_API_MAJOR=tumbler_version_api_major()
TUMBLER_VERSION_API=$TUMBLER_VERSION_API_MAJOR
AC_SUBST([TUMBLER_VERSION_MAJOR])
AC_SUBST([TUMBLER_VERSION_MINOR])
AC_SUBST([TUMBLER_VERSION_MICRO])
AC_SUBST([TUMBLER_VERSION_API_MAJOR])
AC_SUBST([TUMBLER_VERSION_API])

dnl *******************************
dnl *** Check for UNIX variants ***
dnl *******************************
AC_AIX()
AC_ISC_POSIX()
AC_MINIX()

dnl ********************************
dnl *** Check for Win32 variants ***
dnl ********************************
AC_MSG_CHECKING([if building for some Win32 platform])
case "$host" in
*-*-mingw*|*-*-cygwin*)
  ac_bm_platform_win32=yes
  ;;
*)
  ac_bm_platform_win32=no
  ;;
esac
AC_MSG_RESULT([$ac_bm_platform_win32])
AM_CONDITIONAL([PLATFORM_WIN32], [test x"$ac_bm_platform_win32" = x"yes"])

dnl ********************************
dnl *** Check for basic programs ***
dnl ********************************
AM_PROG_AS()
AC_PROG_CC()
AM_PROG_CC_C_O()
AC_PROG_INSTALL()
AC_PROG_INTLTOOL([0.31], [no-xml])
AC_PROG_LIBTOOL()

dnl ***************************************
dnl *** Check for standard header files ***
dnl ***************************************
AC_HEADER_STDC()
AC_CHECK_HEADERS([fcntl.h sys/mman.h sys/stat.h])

dnl ************************************
dnl *** Check for standard functions ***
dnl ************************************
AC_FUNC_MMAP()
#AC_CHECK_FUNCS([])

dnl ******************************
dnl *** Check for i18n support ***
dnl ******************************
dnl XDT_I18N([@LINGUAS@])

dnl *************************
dnl *** Check for GTK-Doc ***
dnl *************************
GTK_DOC_CHECK(1.9)

dnl ***********************************
dnl *** Check for required packages ***
dnl ***********************************

PKG_CHECK_MODULES(GLIB, glib-2.0 >= 2.16.0)
PKG_CHECK_MODULES(GIO, gio-2.0 >= 2.16.0)
PKG_CHECK_MODULES(GTHREAD, gthread-2.0 >= 2.16.0)
PKG_CHECK_MODULES(DBUS, dbus-1 >= 1.0.0)
PKG_CHECK_MODULES(DBUS_GLIB, dbus-glib-1 >= 0.72)

PKG_CHECK_MODULES(PNG, libpng >= 1.2.0, [have_libpng=yes], [
  dnl libpng.pc not found, try with libpng12.pc
  PKG_CHECK_MODULES(PNG, libpng12 >= 1.2.0, 
                    [have_libpng=yes],
                    [jhave_libpng=no])
])
AM_CONDITIONAL([HAVE_LIBPNG], test x"$have_libpng" = x"yes")

dnl ***********************************************
dnl *** Check for the pixbuf thumbnailer plugin ***
dnl ***********************************************
AC_ARG_ENABLE([pixbuf-thumbnailer],
              [AC_HELP_STRING([--enable-pixbuf-thumbnailer],
                              [Enable the pixbuf thumbnailer plugin @<:@default=yes@:>@])],,
              [enable_pixbuf_thumbnailer=yes])

if test x"$enable_pixbuf_thumbnailer" = x"yes"; then
  PKG_CHECK_MODULES(GDK_PIXBUF, 
                    gdk-pixbuf-2.0 >= 2.14,
                    [have_gdkpixbuf=yes], 
                    [have_gdkpixbuf=no])

  if test "x$have_gdkpixbuf" = "xyes"; then
    AC_DEFINE([HAVE_PIXBUF], [1], [Define if the pixbuf thumbnailer plugin is enabled])
  fi
else
  have_gdkpixbuf="no  (disabled)"
fi

AM_CONDITIONAL([ENABLE_PIXBUF_THUMBNAILER], test x"$have_gdkpixbuf" = x"yes")

dnl *********************************************
dnl *** Check for the font thumbnailer plugin ***
dnl *********************************************
AC_ARG_ENABLE([font-thumbnailer],
              [AC_HELP_STRING([--enable-font-thumbnailer],
                              [Enable the font thumbnailer plugin @<:@default=yes@:>@])],,
              [enable_font_thumbnailer=yes])
if test x"$enable_font_thumbnailer" = x"yes"; then
  dnl ******************************
  dnl *** Check for FreeType 2.x ***
  dnl ******************************
  FREETYPE_LIBS=""
  FREETYPE_CFLAGS=""
  AC_PATH_PROG([FREETYPE_CONFIG], [freetype-config], [no])
  if test x"$FREETYPE_CONFIG" != x"no"; then
    AC_MSG_CHECKING([FREETYPE_CFLAGS])
    FREETYPE_CFLAGS="`$FREETYPE_CONFIG --cflags`"
    AC_MSG_RESULT([$FREETYPE_CFLAGS])
  
    AC_MSG_CHECKING([FREETYPE_LIBS])
    FREETYPE_LIBS="`$FREETYPE_CONFIG --libs`"
    AC_MSG_RESULT([$FREETYPE_LIBS])

    AC_DEFINE([ENABLE_FONT_THUMBNAILER], [1], [Define if the font thumbnailer plugin is enabled])
  fi
  AM_CONDITIONAL([HAVE_FREETYPE], [test x"$FREETYPE_CONFIG" != x"no"])
  AC_SUBST([FREETYPE_CFLAGS])
  AC_SUBST([FREETYPE_LIBS])
fi
AM_CONDITIONAL([ENABLE_FONT_THUMBNAILER], [test x"$FREETYPE_CONFIG" != x"no"])

dnl ************************************************************
dnl *** Check for thumbnail flavors (normal, large, cropped) ***
dnl ************************************************************
AC_ARG_ENABLE([normal-thumbnails], 
              [AC_HELP_STRING([--enable-normal-thumbnails], 
                              [Enable normal thumbnails (128 pixels) @<:@default=yes@:>@])],, 
              [enable_normal_thumbnails=yes])
if test x"$enable_normal_thumbnails" = x"yes"; then
  AC_DEFINE([ENABLE_NORMAL_THUMBNAILS], [1], [Define if built with support for normal thumbnails])
fi

AC_ARG_ENABLE([large-thumbnails], 
              [AC_HELP_STRING([--enable-large-thumbnails], 
                              [Enable large thumbnails (256 pixels) @<:@default=no@:>@])],, 
              [enable_large_thumbnails=no])
if test x"$enable_large_thumbnails" = x"yes"; then
  AC_DEFINE([ENABLE_LARGE_THUMBNAILS], [1], [Define if built with support for large thumbnails])
fi

AC_ARG_ENABLE([cropped-thumbnails], 
              [AC_HELP_STRING([--enable-cropped-thumbnails], 
                              [Enable cropped thumbnails (124 pixels) @<:@default=no@:>@])],, 
              [enable_cropped_thumbnails=no])
if test x"$enable_cropped_thumbnails" = x"yes"; then
  AC_DEFINE([ENABLE_CROPPED_THUMBNAILS], [1], [Define if built with support for cropped thumbnails])
fi


dnl ***********************************
dnl *** Check for debugging support ***
dnl ***********************************
dnl XDT_FEATURE_DEBUG()

dnl **************************************
dnl *** Check for linker optimizations ***
dnl **************************************
AC_MSG_CHECKING([whether $LD accepts --as-needed])
case `$LD --as-needed -v 2>&1 </dev/null` in
*GNU* | *'with BFD'*)
  LDFLAGS="$LDFLAGS -Wl,--as-needed"
  AC_MSG_RESULT([yes])
  ;;
*)
  AC_MSG_RESULT([no])
  ;;
esac
AC_MSG_CHECKING([whether $LD accepts -O1])
case `$LD -O1 -v 2>&1 </dev/null` in
*GNU* | *'with BFD'*)
  LDFLAGS="$LDFLAGS -Wl,-O1"
  AC_MSG_RESULT([yes])
  ;;
*)
  AC_MSG_RESULT([no])
  ;;
esac

dnl ****************************************
dnl *** Check for ELF visibility support ***
dnl ****************************************
AC_ARG_ENABLE([visibility], AC_HELP_STRING([--disable-visibility], [Don't use ELF visibility attributes]), [], [enable_visibility=yes])
have_gnuc_visibility=no
if test x"$enable_visibility" != x"no"; then
  dnl Check whether the compiler supports the visibility attribute
  save_CFLAGS="$CFLAGS"
  CFLAGS="$CFLAGS -Wall -Werror"
  AC_MSG_CHECKING([whether $CC supports the GNUC visibility attribute])
  AC_COMPILE_IFELSE(AC_LANG_SOURCE(
  [
    void __attribute__ ((visibility("default"))) test_default (void) {}
    void __attribute__ ((visibility("hidden"))) test_hidden (void) {}
    int main (int argc, char **argv) { test_default (); test_hidden (); return 0; }
  ]),
  [
    have_gnuc_visibility=yes
    AC_MSG_RESULT([yes])
  ],
  [
    AC_MSG_RESULT([no])
  ])
  CFLAGS="$save_CFLAGS"
fi
if test x"$have_gnuc_visibility" = x"yes"; then
  CPPFLAGS="$CPPFLAGS -DHAVE_GNUC_VISIBILITY"
fi
AM_CONDITIONAL([HAVE_GNUC_VISIBILITY], [test x"$have_gnuc_visibility" = x"yes"])

AC_OUTPUT([
Makefile
docs/Makefile
docs/reference/Makefile
docs/reference/tumbler/Makefile
docs/reference/tumbler/version.xml
plugins/Makefile
plugins/font-thumbnailer/Makefile
plugins/pixbuf-thumbnailer/Makefile
plugins/xdg-cache/Makefile
po/Makefile.in
tumbler/Makefile
tumbler/tumbler-1.pc
tumbler/tumbler-config.h
tumblerd/Makefile
])

dnl ***************************
dnl *** Print configuration ***
dnl ***************************
echo
echo "Build Configuration:"
echo
echo "  * Debug:                       $enable_debug"
if test x"$enable_pixbuf_thumbnailer" = x"yes"; then
echo "  * Pixbuf thumbnailer plugin:   yes"
else
echo "  * Pixbuf thumbnailer plugin:   no"
fi
if test x"$enable_font_thumbnailer" = x"yes"; then
echo "  * Font thumbnailer plugin:     yes"
else
echo "  * Font thumbnailer plugin:     no"
fi
echo
echo "Supported Thumbnail Flavors:"
echo
if test x"$enable_normal_thumbnails" = x"yes"; then
echo "  * Normal (128 pixels):         yes"
else
echo "  * Normal (128 pixels):         no"
fi
if test x"$enable_large_thumbnails" = x"yes"; then
echo "  * Large (256 pixels):          yes"
else
echo "  * Large (256 pixels):          no"
fi
if test x"$enable_cropped_thumbnails" = x"yes"; then
echo "  * Cropped (124 pixels):        yes"
else
echo "  * Cropped (124 pixels):        no"
fi
echo