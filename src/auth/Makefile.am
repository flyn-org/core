auth_moduledir = $(moduledir)/auth

# automake seems to force making this unconditional..
NOPLUGIN_LDFLAGS =

if GSSAPI_PLUGIN
GSSAPI_LIB = libmech_gssapi.la
endif

if HAVE_LDAP
if LDAP_PLUGIN
LDAP_LIB = libauthdb_ldap.la
endif
endif

LUA_LIB =
AUTH_LUA_LIBS =
AUTH_LUA_LDADD =

if HAVE_LUA
if AUTH_LUA_PLUGIN
LUA_LIB += libauthdb_lua.la
else
AUTH_LUA_LIBS += $(LIBDOVECOT_LUA)
AUTH_LUA_LDADD += $(LUA_LIBS)
endif
endif

auth_module_LTLIBRARIES = \
	$(GSSAPI_LIB) \
	$(LDAP_LIB) \
	$(LUA_LIB) \
	libauthdb_imap.la

pkglibexecdir = $(libexecdir)/dovecot

pkglibexec_PROGRAMS = auth

AM_CPPFLAGS = \
	-I$(top_srcdir)/src/lib \
	-I$(top_srcdir)/src/lib-auth \
	-I$(top_srcdir)/src/lib-auth-client \
	-I$(top_srcdir)/src/lib-test \
	-I$(top_srcdir)/src/lib-json \
	-I$(top_srcdir)/src/lib-ldap \
	-I$(top_srcdir)/src/lib-dict \
	-I$(top_srcdir)/src/lib-dns-client \
	-I$(top_srcdir)/src/lib-http \
	-I$(top_srcdir)/src/lib-sql \
	-I$(top_srcdir)/src/lib-settings \
	-I$(top_srcdir)/src/lib-otp \
	-I$(top_srcdir)/src/lib-master \
	-I$(top_srcdir)/src/lib-login \
	-I$(top_srcdir)/src/lib-oauth2 \
	-I$(top_srcdir)/src/lib-ssl-iostream \
	-I$(top_srcdir)/src/lib-lua \
	-I$(top_srcdir)/src/lib-dcrypt \
	-I$(top_srcdir)/src/lib-var-expand \
	-DAUTH_MODULE_DIR=\""$(auth_moduledir)"\" \
	-DPKG_LIBEXECDIR=\""$(pkglibexecdir)"\" \
	-DPKG_RUNDIR=\""$(rundir)"\" \
	-DSYSCONFDIR=\""$(sysconfdir)/dovecot"\" \
	$(LUA_CFLAGS) \
	$(AUTH_CFLAGS)

auth_LDFLAGS = -export-dynamic

auth_libs = \
	../lib-auth/libauth-crypt.la \
	$(AUTH_LUA_LIBS) \
	$(LIBDOVECOT_SQL)

auth_CPPFLAGS = $(AM_CPPFLAGS) $(BINARY_CFLAGS)
auth_LDADD = $(auth_libs) $(LIBDOVECOT) $(AUTH_LIBS) $(BINARY_LDFLAGS) $(AUTH_LUA_LDADD)
auth_DEPENDENCIES = $(auth_libs) $(LIBDOVECOT_DEPS)
auth_SOURCES = main.c $(auth_common_sources)

ldap_sources = db-ldap.c db-ldap-settings.c passdb-ldap.c userdb-ldap.c
lua_sources = db-lua.c passdb-lua.c userdb-lua.c

auth_common_sources = \
	auth.c \
	auth-cache.c \
	auth-client-connection.c \
	auth-master-connection.c \
	auth-policy.c \
	mech-otp-common.c \
	mech-plain-common.c \
	auth-penalty.c \
	auth-request.c \
	auth-request-fields.c \
	auth-request-handler.c \
	auth-request-var-expand.c \
	auth-settings.c \
	auth-fields.c \
	auth-token.c \
	auth-worker-connection.c \
	auth-worker-server.c \
	db-oauth2.c \
	db-sql.c \
	db-passwd-file.c \
	mech.c \
	mech-anonymous.c \
	mech-plain.c \
	mech-login.c \
	mech-cram-md5.c \
	mech-digest-md5.c \
	mech-external.c \
	mech-gssapi.c \
	mech-otp.c \
	mech-scram.c \
	mech-apop.c \
	mech-winbind.c \
	mech-dovecot-token.c \
	mech-oauth2.c \
	passdb.c \
	passdb-blocking.c \
	passdb-bsdauth.c \
	passdb-cache.c \
	passdb-oauth2.c \
	passdb-passwd.c \
	passdb-passwd-file.c \
	passdb-pam.c \
	passdb-sql.c \
	passdb-static.c \
	userdb.c \
	userdb-blocking.c \
	userdb-passwd.c \
	userdb-passwd-file.c \
	userdb-prefetch.c \
	userdb-static.c \
	userdb-sql.c \
if HAVE_LDAP
	$(ldap_sources) \
endif
if HAVE_LUA
	$(lua_sources)
endif

headers = \
	auth.h \
	auth-cache.h \
	auth-client-connection.h \
	auth-common.h \
	auth-master-connection.h \
	mech-otp-common.h \
	mech-plain-common.h \
	mech-digest-md5-private.h \
	mech-scram.h \
	auth-penalty.h \
	auth-policy.h \
	auth-request.h \
	auth-request-handler.h \
	auth-request-handler-private.h \
	auth-request-var-expand.h \
	auth-settings.h \
	auth-fields.h \
	auth-token.h \
	auth-worker-connection.h \
	auth-worker-server.h \
	db-ldap.h \
	db-ldap-settings.h \
	db-sql.h \
	db-passwd-file.h \
	db-oauth2.h \
	mech.h \
	passdb.h \
	passdb-blocking.h \
	passdb-cache.h \
	userdb.h \
	userdb-blocking.h

if GSSAPI_PLUGIN
libmech_gssapi_la_LDFLAGS = -module -avoid-version
libmech_gssapi_la_LIBADD = $(KRB5_LIBS)
libmech_gssapi_la_CPPFLAGS = $(AM_CPPFLAGS) $(KRB5_CFLAGS) -DPLUGIN_BUILD
libmech_gssapi_la_SOURCES = mech-gssapi.c
endif

if HAVE_LDAP
if LDAP_PLUGIN
libauthdb_ldap_la_LDFLAGS = -module -avoid-version -shared
libauthdb_ldap_la_LIBADD = $(LIBDOVECOT_LDAP) $(LDAP_LIBS)
libauthdb_ldap_la_CPPFLAGS = $(AM_CPPFLAGS) -DPLUGIN_BUILD
libauthdb_ldap_la_SOURCES = $(ldap_sources)
else
auth_libs += $(LIBDOVECOT_LDAP)
endif
endif

if AUTH_LUA_PLUGIN
libauthdb_lua_la_LDFLAGS = -module -avoid-version -shared
libauthdb_lua_la_LIBADD = $(LIBDOVECOT_LUA)
libauthdb_lua_la_CPPFLAGS = $(AM_CPPFLAGS) -DPLUGIN_BUILD
libauthdb_lua_la_SOURCES = $(lua_sources)
endif

libauthdb_imap_la_LDFLAGS = -module -avoid-version -shared
libauthdb_imap_la_LIBADD = \
	../lib-imap-client/libimap_client.la \
	$(LIBDOVECOT)
libauthdb_imap_la_CPPFLAGS = \
	$(AM_CPPFLAGS) \
	-I$(top_srcdir)/src/lib-imap \
	-I$(top_srcdir)/src/lib-imap-client
libauthdb_imap_la_SOURCES = passdb-imap.c

pkginc_libdir=$(pkgincludedir)
pkginc_lib_HEADERS = $(headers)

test_programs = \
	test-auth-cache \
	test-auth-client \
	test-auth-master \
	test-auth \
	test-mech

noinst_PROGRAMS = $(test_programs)

noinst_HEADERS = test-auth.h db-lua.h test-auth-master.h

test_auth_cache_SOURCES = auth-cache.c test-auth-cache.c
test_auth_cache_LDADD = $(LIBDOVECOT)
test_auth_cache_DEPENDENCIES = $(pkglibexec_PROGRAMS) $(LIBDOVECOT_DEPS)
# this is needed to force auth-cache.c recompilation
test_auth_cache_CPPFLAGS = $(AM_CPPFLAGS)

test_auth_SOURCES = \
	$(auth_common_sources) \
	test-auth-request-var-expand.c \
	test-auth-request-fields.c \
	test-username-filter.c \
if HAVE_LUA
	test-lua.c \
endif
	test-mock.c \
	test-main.c

test_auth_LDADD = $(LIBDOVECOT) $(auth_libs) $(AUTH_LIBS) $(LUA_LIBS)
test_auth_DEPENDENCIES = $(pkglibexec_PROGRAMS) $(LIBDOVECOT_DEPS)

test_mech_SOURCES = \
	$(auth_common_sources) \
	test-auth.c \
	test-mock.c \
	test-mech.c

test_mech_LDADD = $(LIBDOVECOT) $(auth_libs) $(AUTH_LIBS) $(LUA_LIBS)
test_mech_DEPENDENCIES = $(pkglibexec_PROGRAMS) $(LIBDOVECOT_DEPS)

test_auth_client_SOURCES = \
	$(auth_common_sources) \
	test-auth.c \
	test-mock.c \
	test-auth-client.c

test_auth_client_LDADD = $(LIBDOVECOT) $(auth_libs) $(AUTH_LIBS) $(LUA_LIBS)
test_auth_client_DEPENDENCIES = $(pkglibexec_PROGRAMS) $(LIBDOVECOT_DEPS)

test_auth_master_SOURCES = \
	$(auth_common_sources) \
	test-auth.c \
	test-mock.c \
	test-auth-master.c \
	test-auth-master-server.c

test_auth_master_LDADD = $(LIBDOVECOT) $(auth_libs) $(AUTH_LIBS) $(LUA_LIBS)
test_auth_master_DEPENDENCIES = $(pkglibexec_PROGRAMS) $(LIBDOVECOT_DEPS)


check-local:
	for bin in $(test_programs); do \
	  if ! $(RUN_TEST) ./$$bin; then exit 1; fi; \
	done
