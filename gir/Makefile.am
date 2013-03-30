GL_SPECS = gl.spec gl.tm enum.spec enumext.spec
GL_NSVERSION = 1.0
GL_HEADER = gl.h
GL_GIR = GLEW-$(GL_NSVERSION).gir
GL_TYPELIB = GLEW-$(GL_NSVERSION).typelib
GL_VAPI = gl.vapi

girdir = $(datadir)/gir-1.0
gir_DATA = $(GL_GIR)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(GL_TYPELIB)

vapidir = $(datadir)/vala/vapi
vapi_DATA = $(GL_VAPI)

noinst_PROGRAMS = generate

generate_SOURCES = generate.vala
generate_VALAFLAGS = $(GENERATE_VALAPACKAGES)
generate_CPPFLAGS = $(GLIB_CFLAGS) $(GEE_CFLAGS)
generate_LDADD = $(GLIB_LIBS) $(GEE_LIBS)


sync: $(GL_SPECS)

gl.spec:
	wget http://www.opengl.org/registry/api/gl.spec

gl.tm:
	wget http://www.opengl.org/registry/api/gl.tm

enum.spec:
	wget http://www.opengl.org/registry/api/enum.spec

enumext.spec:
	wget http://www.opengl.org/registry/api/enumext.spec

$(GL_HEADER): $(GL_SPECS) generate
	./generate --header -h $(GL_HEADER) $(GL_SPECS)

$(GL_GIR): $(GL_HEADER) generate
	g-ir-scanner -v --warn-all --no-libtool --external-library -n "GLEW" --nsversion "$(GL_NSVERSION)" --symbol-prefix "" --identifier-prefix "" --accept-unprefixed --pkg=glew --c-include "GL/glew.h" --library "GLEW" --header-only -o "$(GL_GIR)" "$(GL_HEADER)"
	./generate --fix-gir -g $(GL_GIR) $(GL_GIR)

$(GL_TYPELIB): $(GL_GIR)
	g-ir-compiler --includedir=. $(GL_GIR) -o $(GL_TYPELIB)

$(GL_VAPI): $(GL_GIR)
	vapigen $(GL_GIR) --library=gl

clean-local:
	-rm -rf tmp-introspect*

CLEANFILES = *.h *.c *.vapi *.stamp $(GL_HEADER) $(GL_GIR) parse fix#$(GL_TYPELIB)

#EXTRA_DIST = GL-custom.h Glut-custom.h GLTypes-1.0.gir
