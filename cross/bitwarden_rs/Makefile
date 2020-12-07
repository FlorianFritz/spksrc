PKG_NAME = bitwarden_rs
PKG_VERS = 1.17.0
PKG_EXT = tar.gz
PKG_DIST_NAME = $(PKG_VERS).$(PKG_EXT)
PKG_DIST_SITE = https://github.com/dani-garcia/bitwarden_rs/archive/
PKG_DIR = $(PKG_NAME)-$(PKG_VERS)

DEPENDS = cross/openssl cross/sqlite

HOMEPAGE = https://github.com/dani-garcia/bitwarden_rs
COMMENT  = This is a Bitwarden server API implementation written in Rust compatible with upstream Bitwarden clients (https://bitwarden.com/#download).
LICENSE  = GPLv3


GNU_CONFIGURE = 0
CONFIGURE_ARGS = 
PRE_CONFIGURE_TARGET = 
CONFIGURE_TARGET = rust_setup
POST_CONFIGURE_TARGET = 

#RUSTUP_HOME = $(WORK_DIR)
#CARGO_HOME = $(WORK_DIR)/cargo
RUSTUP_HOME = ${HOME}/.rustup
CARGO_HOME = ${HOME}/.cargo
$(warning "RUSTUP_HOME $(RUSTUP_HOME)")
$(warning "CARGO_HOME $(CARGO_HOME)")

CURL_BIN = $(shell which curl)
ifeq (,$(CURL_BIN))
exit 1
endif

RUST_TARGET = $(ARCH)-unknown-linux-gnu
ifeq ($(ARCH),armada370)
RUST_TARGET = "armv7-unknown-linux-gnueabihf"
endif

RUSTUP_BIN = RUSTUP_HOME=$(RUSTUP_HOME) CARGO_HOME=$(CARGO_HOME) $(CARGO_HOME)/bin/rustup
CARGO_BIN = RUSTUP_HOME=$(RUSTUP_HOME) CARGO_HOME=$(CARGO_HOME) $(CARGO_HOME)/bin/cargo
$(warning "RUSTUP_BIN $(RUSTUP_BIN)")
$(warning "CARGO_BIN $(CARGO_BIN)")

#PRE_COMPILE_TARGET = rust_setup
COMPILE_TARGET = rust_compile
#INSTALL_TARGET = rust_install
#POST_COMPILE_TARGET = 


#ifeq ($(findstring $(ARCH), ppc824x ppc853x),$(ARCH))
#ifneq (5,$(firstword $(sort $(TCVERSION) 5)))
## toolchains before version 5 require the old linux quota version
## related issue: https://github.com/transmission/transmission/issues/591
#ADDITIONAL_CFLAGS = -D_LINUX_QUOTA_VERSION=1
#endif
#endif

include ../../mk/spksrc.cross-cc.mk

.PHONY: rust_setup
rust_setup:
	@$(MSG) "Get Rust for $(NAME)"
	@mkdir -p $(CARGO_HOME)
	$(CURL_BIN) --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs -o $(WORK_DIR)/rustup-init.sh
	@chmod +x $(WORK_DIR)/rustup-init.sh
	@RUSTUP_HOME=$(RUSTUP_HOME) CARGO_HOME=$(CARGO_HOME) sh $(WORK_DIR)/rustup-init.sh -y --no-modify-path
	@$(MSG) "Installing RUST_TARGET: $(RUST_TARGET)"
	@$(RUN) $(RUSTUP_BIN) target add $(RUST_TARGET)

.PHONY: rust_compile
rust_compile: 
	@$(MSG) "Rust compile $(NAME)"
	#@export TARGET_CC=$(TC_PATH)/$(TC_PREFIX)gcc export OPENSSL_DIR=$(STAGING_INSTALL_PREFIX) && cd $(WORK_DIR)/$(PKG_DIR) && $(CARGO_BIN) build --features sqlite --release --target $(RUST_TARGET)
	@cd $(WORK_DIR)/$(PKG_DIR) && TARGET_CC=$(TC_PATH)/$(TC_PREFIX)gcc OPENSSL_DIR=$(STAGING_INSTALL_PREFIX) $(CARGO_BIN) build --features sqlite --release --target $(RUST_TARGET)

.PHONY: rust_install
rust_install: 
	@$(MSG) "Rust install $(NAME)"
	#@export OPENSSL_DIR=$(STAGING_INSTALL_PREFIX)  && $(CARGO_BIN) build --features sqlite --release --target $(RUST_TARGET)
