PKG_NAME = mysql-connector-c
PKG_VERS = 6.1.11
PKG_EXT = tar.gz
DIST_NAME = $(PKG_NAME)-$(PKG_VERS)-src
PKG_DIST_NAME = $(DIST_NAME).$(PKG_EXT)
PKG_DIST_SITE = https://cdn.mysql.com/archives/$(PKG_NAME)
PKG_DIR = $(DIST_NAME)

BUILD_DEPENDS = native/libmysqlclient

HOMEPAGE = https://downloads.mysql.com/archives/c-c
COMMENT  = MySQL C API (libmysqlclient) 
LICENSE  = GNU GPLv2

include ../../mk/spksrc.cross-cmake.mk

# Mandatory for build
CMAKE_ARGS += -DHAVE_LLVM_LIBCPP_EXITCODE=1 
CMAKE_ARGS += -DHAVE_CXX_FLOATING_POINT_OPTIMIZATION_PROBLEMS_EXITCODE=1
CMAKE_ARGS += -DHAVE_C_FLOATING_POINT_OPTIMIZATION_PROBLEMS_EXITCODE=1
CMAKE_ARGS += -DHAVE_C_FLOATING_POINT_OPTIMIZATION_PROBLEMS_EXITCODE__TRYRUN_OUTPUT=1
# MySQL specific:
CMAKE_ARGS += -DSTACK_DIRECTION=1
CMAKE_ARGS += -DWITH_UNIT_TESTS=OFF
CMAKE_ARGS += -DWITH_EMBEDDED_SERVER=FALSE
CMAKE_ARGS += -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci
# Fix full lib version:
CMAKE_ARGS += -DSHARED_LIB_PATCH_VERSION=0
# Fix for mariadb connector
CMAKE_ARGS += -DINSTALL_INCLUDEDIR=include/mysql

# WITH_SSL=[yes|bundled|system|<path/to/custom/installation>]
# Not compatible with OpenSSL 1.1
# Requires updating to mysql-connector-c++ version 8.x
#DEPENDS += cross/openssl
#CMAKE_ARGS += -DWITH_SSL=system
DEPENDS += cross/zlib
CMAKE_ARGS += -DWITH_ZLIB=system

# Use gcc builtin atomic functions for ppc architectures
ifneq ($(findstring $(ARCH),$(PPC_ARCHS)),)
CMAKE_ARGS += -DHAVE_GCC_ATOMIC_BUILTINS=1
endif

# give access to comp_err binary on host
ENV += PATH=$(WORK_DIR)/../../../native/libmysqlclient/work-native/install/usr/local/bin/:$$PATH
