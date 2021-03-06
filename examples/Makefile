OS := $(shell uname)

SOURCE_DIR = ../src
BUILD_DIR = $(CURDIR)/build
LIB_DIR = $(BUILD_DIR)/debug
INCLUDE_DIR = ../include

CFLAGS = -I. -Wall -Werror -pedantic -fsanitize=address

ifeq ($(OS), Darwin)
CFLAGS += -framework Security
endif

LIBCRYPTO_DIR = $(dir $(shell find ${BUILD_DIR} -name libcrypto.a))
LIBSSL_DIR = $(dir $(shell find ${BUILD_DIR} -name libssl.a))

LDFLAGS = -L$(LIBCRYPTO_DIR) -L$(LIBSSL_DIR) -L$(LIB_DIR)

LIBS = -lcrypto -lssl -lquiche -lev -ldl -pthread

all: client server

client: client.c $(INCLUDE_DIR)/quiche.h $(LIB_DIR)/libquiche.a
	$(CC) $(CFLAGS) $(LDFLAGS) $< -o $@ -I$(INCLUDE_DIR) $(LIBS)

server: server.c $(INCLUDE_DIR)/quiche.h $(LIB_DIR)/libquiche.a
	$(CC) $(CFLAGS) $(LDFLAGS) $< -o $@ -I$(INCLUDE_DIR) $(LIBS)

$(LIB_DIR)/libquiche.a: $(SOURCE_DIR)/*.rs
	cd .. && cargo build --target-dir $(BUILD_DIR)
