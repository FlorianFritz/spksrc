SPK_NAME   = bitwarden_rs
SPK_VERS   = 1.17.0
SPK_REV    = 1
SPK_ICON   = src/bitwarden.png
DSM_UI_DIR = app

BETA = 1

DEPENDS = cross/$(SPK_NAME)

MAINTAINER = Florian Fritz
DESCRIPTION = This is a Bitwarden server API implementation written in Rust compatible with upstream Bitwarden clients (https://bitwarden.com/#download).
ADMIN_URL = /bitwarden/
RELOAD_UI = yes
DISPLAY_NAME = Bitwarden (Rust Edition)
CHANGELOG = "1) Initial Package Release"

HOMEPAGE   = https://github.com/dani-garcia/bitwarden_rs
LICENSE    = GPLv3

REQUIRED_DSM = 6.0

#WIZARDS_DIR = src/wizard/
#CONF_DIR = src/conf/

INSTALLER_SCRIPT = src/installer.sh

SERVICE_USER         = auto
SERVICE_WIZARD_GROUP = wizard_group
SERVICE_WIZARD_SHARE = wizard_download_dir
SERVICE_SETUP        = src/service-setup.sh
STARTABLE            = yes

# Service configuration
SERVICE_PORT = 8001
SERVICE_PORT_TITLE = Bitwarden (Rust)

# Admin link
ADMIN_PORT = $(SERVICE_PORT)

#INSTALL_DEP_SERVICES = apache-web mysql
#START_DEP_SERVICES = apache-web mysql
#INSTUNINST_RESTART_SERVICES = apache-web

#INSTALL_PREFIX = /usr/local/$(SPK_NAME)

#POST_STRIP_TARGET = owncloud_extra_install

#BUSYBOX_CONFIG = daemon nice
#ENV += BUSYBOX_CONFIG="$(BUSYBOX_CONFIG)"

include ../../mk/spksrc.spk.mk

#.PHONY: owncloud_extra_install
#owncloud_extra_install:
#	install -m 755 -d $(STAGING_DIR)/var
#	install -m 755 src/owncloud.sh $(STAGING_DIR)/bin/
#	install -m 755 -d $(STAGING_DIR)/app
#	install -m 644 src/app/config $(STAGING_DIR)/app/config
#	install -m 644 src/autoconfig.php $(STAGING_DIR)/share/$(SPK_NAME)/config/autoconfig.php
#