lib_LTLIBRARIES = libwnck-1.la
bin_PROGRAMS =			\
	wnckprop		\
	wnck-urgency-monitor
noinst_PROGRAMS =		\
	test-pager		\
	test-wnck		\
	test-selector		\
	test-tasklist		\
	test-urgent

AM_CPPFLAGS =					\
	$(LIBWNCK_CFLAGS)			\
	-I$(top_srcdir)				\
	-DWNCK_I_KNOW_THIS_IS_UNSTABLE		\
	-DWNCK_LOCALEDIR=\"$(wncklocaledir)\"	\
	$(DISABLE_DEPRECATED_CFLAGS)

AM_CFLAGS = $(WARN_CFLAGS)

wnck_headers =			\
	libwnck.h		\
	pager.h			\
	application.h		\
	class-group.h		\
	screen.h		\
	selector.h		\
	tasklist.h		\
	util.h			\
	window.h 		\
	window-action-menu.h	\
	workspace.h

wnck_built_installed_headers = wnck-enum-types.h

libwnckincludedir = $(includedir)/libwnck-1.0/libwnck
libwnckinclude_HEADERS =		\
	$(wnck_headers)			\
	$(wnck_built_installed_headers)

# Sources that are relevant for introspection
wnck_sources =			\
	application.c		\
	class-group.c		\
	pager.c			\
	screen.c		\
	selector.c		\
	tasklist.c		\
	util.c			\
	window.c		\
	window-action-menu.c	\
	workspace.c

wnck_accessibility_files =			\
	pager-accessible.c			\
	pager-accessible.h			\
	pager-accessible-factory.c		\
	pager-accessible-factory.h		\
	workspace-accessible.c			\
	workspace-accessible.h			\
	workspace-accessible-factory.c		\
	workspace-accessible-factory.h

wnck_built_headers = $(wnck_built_installed_headers) wnck-marshal.h inlinepixbufs.h
wnck_built_cfiles = wnck-enum-types.c wnck-marshal.c

libwnck_1_la_SOURCES = 		\
	$(wnck_built_headers)	\
	$(wnck_built_cfiles)	\
	$(wnck_sources)		\
	private.h		\
	xutils.c		\
	xutils.h		\
	$(wnck_accessibility_files)

$(libwnck_1_la_OBJECTS): $(wnck_built_headers)

libwnck_1_la_CPPFLAGS =			\
	$(AM_CPPFLAGS)			\
	$(XLIB_CFLAGS)			\
	-DG_LOG_DOMAIN=\"Wnck\"		\
	-DSN_API_NOT_YET_FROZEN=1

libwnck_1_la_LIBADD =	\
	$(LIBWNCK_LIBS)	\
	$(XLIB_LIBS)	\
	$(LIBM)

libwnck_1_la_LDFLAGS =								\
	-version-info $(LIBWNCK_CURRENT):$(LIBWNCK_REVISION):$(LIBWNCK_AGE)	\
	-no-undefined								\
	-export-symbols-regex "^[^_].*"

wnckprop_SOURCES = wnckprop.c
wnck_urgency_monitor_SOURCES = wnck-urgency-monitor.c
test_wnck_SOURCES = test-wnck.c
test_tasklist_SOURCES = test-tasklist.c
test_selector_SOURCES = test-selector.c
test_pager_SOURCES = test-pager.c
test_urgent_SOURCES = test-urgent.c

wnckprop_LDADD = $(LIBWNCK_LIBS) ./$(lib_LTLIBRARIES)
wnck_urgency_monitor_LDADD = $(LIBWNCK_LIBS) ./$(lib_LTLIBRARIES)
test_wnck_LDADD = $(LIBWNCK_LIBS) ./$(lib_LTLIBRARIES)
test_tasklist_LDADD = $(LIBWNCK_LIBS) ./$(lib_LTLIBRARIES)
test_selector_LDADD = $(LIBWNCK_LIBS) ./$(lib_LTLIBRARIES)
test_pager_LDADD = $(LIBWNCK_LIBS) ./$(lib_LTLIBRARIES)
test_urgent_LDADD = $(LIBWNCK_LIBS)

wnck-marshal.h: wnck-marshal.list
	$(AM_V_GEN)$(GLIB_GENMARSHAL) --prefix=_wnck_marshal $< --header > $@

wnck-marshal.c: wnck-marshal.list wnck-marshal.h
	$(AM_V_GEN)echo "#include <libwnck/libwnck.h>" > $@ \
	&& echo "#include \"wnck-marshal.h\"" >> $@ \
	&& $(GLIB_GENMARSHAL) --prefix=_wnck_marshal $< --body >> $@

wnck-enum-types.h: $(wnck_headers)
	$(AM_V_GEN)glib-mkenums \
		--fhead "#ifndef __WNCK_ENUM_TYPES_H__\n" \
		--fhead "#define __WNCK_ENUM_TYPES_H__\n\n" \
		--fhead "G_BEGIN_DECLS\n\n" \
		--ftail "G_END_DECLS\n\n" \
		--ftail "#endif /* __WNCK_ENUM_TYPES_H__ */\n" \
		--fprod "\n/* enumerations from \"@filename@\" */\n" \
		--eprod "#define WNCK_TYPE_@ENUMSHORT@ @enum_name@_get_type()\n" \
		--eprod "GType @enum_name@_get_type (void);\n" \
		$^ > $@

wnck-enum-types.c: $(wnck_headers)
	$(AM_V_GEN)glib-mkenums \
		--fhead "#include <libwnck/libwnck.h>\n" \
		--fprod "\n/* enumerations from \"@filename@\" */" \
		--vhead "static const G@Type@Value _@enum_name@_values[] = {" \
		--vprod "  { @VALUENAME@, \"@VALUENAME@\", \"@valuenick@\" }," \
		--vtail "  { 0, NULL, NULL }\n};\n\n" \
		--vtail "GType\n@enum_name@_get_type (void)\n{\n" \
		--vtail "  static GType type = 0;\n\n" \
		--vtail "  if (!type)\n" \
		--vtail "    type = g_@type@_register_static (\"@EnumName@\", _@enum_name@_values);\n\n" \
		--vtail "  return type;\n}\n\n" \
		$^ > $@

inlinepixbufs_SOURCES = default_icon.png stock_delete.png stock_maximize.png stock_minimize.png
inlinepixbufs_pairs =						\
	default_icon_data $(srcdir)/default_icon.png 		\
	stock_delete_data $(srcdir)/stock_delete.png 		\
	stock_maximize_data $(srcdir)/stock_maximize.png 	\
	stock_minimize_data $(srcdir)/stock_minimize.png

inlinepixbufs.h: $(inlinepixbufs_SOURCES)
	$(AM_V_GEN)$(GDK_PIXBUF_CSOURCE) --raw --build-list $(inlinepixbufs_pairs) > $@

CLEANFILES =			\
	$(wnck_built_cfiles)	\
	$(wnck_built_headers)

EXTRA_DIST = 				\
	$(inlinepixbufs_SOURCES)	\
	wnck-marshal.list

# Introspection
-include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ARGS = --warn-all --add-include-path=$(srcdir)
INTROSPECTION_COMPILER_ARGS = --includedir=$(srcdir)

if HAVE_INTROSPECTION
introspection_sources = $(wnck_sources) $(libwnckinclude_HEADERS)

Wnck-1.0.gir: libwnck-1.la
Wnck_1_0_gir_INCLUDES = GObject-2.0 GdkPixbuf-2.0 Gtk-2.0
Wnck_1_0_gir_CFLAGS = $(AM_CPPFLAGS)
Wnck_1_0_gir_LIBS = libwnck-1.la
Wnck_1_0_gir_SCANNERFLAGS = --pkg-export=libwnck-1.0
Wnck_1_0_gir_FILES = $(introspection_sources)
INTROSPECTION_GIRS += Wnck-1.0.gir

girdir = $(INTROSPECTION_GIRDIR)
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(INTROSPECTION_TYPELIBDIR)
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

CLEANFILES += $(gir_DATA) $(typelib_DATA)
endif

-include $(top_srcdir)/git.mk
