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

################################################################################
#                                  RUST SETUP                                  #
################################################################################

#RUSTUP_HOME = $(WORK_DIR)
#CARGO_HOME = $(WORK_DIR)/cargo
RUSTUP_HOME = ${HOME}/.rustup
CARGO_HOME = ${HOME}/.cargo
#$(warning "RUSTUP_HOME $(RUSTUP_HOME)")
#$(warning "CARGO_HOME $(CARGO_HOME)")

CURL_BIN = $(shell which curl)
ifeq (,$(CURL_BIN))
exit 1
endif

$(warning "ARCH $(ARCH)")

RUST_TARGET = $(ARCH)-unknown-linux-gnu
ifeq ($(ARCH),armada370)
RUST_TARGET = armv7-unknown-linux-gnueabihf
endif
$(warning "RUST_TARGET $(RUST_TARGET)")

CARGO_TARGET_LINKER = $(shell echo "CARGO_TARGET_$(subst -,_,$(RUST_TARGET))_LINKER" | sed 's/./\U&/g')
$(warning "CARGO_TARGET_LINKER $(CARGO_TARGET_LINKER)")

RUSTUP_BIN = RUSTUP_HOME=$(RUSTUP_HOME) CARGO_HOME=$(CARGO_HOME) $(CARGO_HOME)/bin/rustup
CARGO_BIN = RUSTUP_HOME=$(RUSTUP_HOME) CARGO_HOME=$(CARGO_HOME) $(CARGO_HOME)/bin/cargo
$(warning "RUSTUP_BIN $(RUSTUP_BIN)")
$(warning "CARGO_BIN $(CARGO_BIN)")

CARGO_CONFIG_DIR = $(WORK_DIR)/.cargo
CARGO_CONFIG_FILE = $(CARGO_CONFIG_DIR)/config.toml

################################################################################
#                                   CONFIGURE                                  #
################################################################################
GNU_CONFIGURE = 0
CONFIGURE_ARGS = 
PRE_CONFIGURE_TARGET = 
CONFIGURE_TARGET = rust_setup
POST_CONFIGURE_TARGET = 

################################################################################
#                                   COMPILE                                    #
################################################################################
#PRE_COMPILE_TARGET = 
COMPILE_TARGET = rust_compile
#POST_COMPILE_TARGET = 

################################################################################
#                                   INSTALL                                    #
################################################################################
INSTALL_TARGET = rust_install

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
	@mkdir -p $(CARGO_CONFIG_DIR)
	@touch $(CARGO_CONFIG_FILE)
	@echo "[target.$(RUST_TARGET)]" > $(CARGO_CONFIG_FILE)
	@echo "linker = \"$(TC_PATH)$(TC_PREFIX)gcc\"" >> $(CARGO_CONFIG_FILE)
	@cd $(WORK_DIR)/$(PKG_DIR) && TARGET_CC=$(TC_PATH)$(TC_PREFIX)gcc $(CARGO_TARGET_LINKER)=$(TC_PATH)$(TC_PREFIX)gcc OPENSSL_DIR=$(STAGING_INSTALL_PREFIX) $(CARGO_BIN) build --features sqlite --release --target $(RUST_TARGET) --verbose

.PHONY: rust_install
rust_install: 
	@$(MSG) "Rust install $(NAME)"
	#install -m 755 -d $(STAGING_DIR)/var
	#install -m 644 src/settings.json $(STAGING_DIR)/var/settings.json
	install -m 755 $(WORK_DIR)/$(PKG_DIR)/target/$(RUST_TARGET)/release/$(PKG_NAME) $(STAGING_INSTALL_PREFIX)/bin

