# Basic Settings
SHELL           := /bin/bash
TOP_DIR         ?= $(CURDIR)/../
SUBDIRS         := directory-not-exist-actually

# Settings of input directory
SCRIPT_DIR      := $(TOP_DIR)/tools/build_scripts

include $(TOP_DIR)/make.settings
include $(SCRIPT_DIR)/parse_make_settings.mk

# Makefile echo
ifeq ($(DEBUG_MAKEFILE),n)
    Q               := @
    TOP_Q           := @
else
    Q               :=
    TOP_Q           :=
endif

# Settings of output directory
SAMPLE_DIR		:= $(CURDIR)
FINAL_DIR       := $(CURDIR)/../output/$(BUILD_TYPE)

IOT_LIB_DIR = $(FINAL_DIR)/lib
IOT_INC_CFLAGS = -I$(FINAL_DIR)/include -I$(FINAL_DIR)/include/exports  -I$(CURDIR)/../sdk_src/internal_inc

LDFLAGS             := -Wl,--start-group $(IOT_LIB_DIR)/libiot_sdk.a
ifeq ($(FEATURE_AUTH_WITH_NOTLS),n)
LDFLAGS             += $(IOT_LIB_DIR)/libmbedtls.a $(IOT_LIB_DIR)/libmbedx509.a $(IOT_LIB_DIR)/libmbedcrypto.a
endif
LDFLAGS             += $(IOT_LIB_DIR)/libiot_platform.a -Wl,--end-group

ifeq ($(FEATURE_WIFI_CONFIG_ENABLED),y)
LDFLAGS += $(IOT_LIB_DIR)/libcjson.a
endif

CFLAGS += -Wall -Wno-error=sign-compare -Wno-error=format -Os -pthread -DFORCE_SSL_VERIFY
CFLAGS += ${IOT_INC_CFLAGS}

ifeq ($(FEATURE_AUTH_MODE),CERT)
CFLAGS += -DAUTH_MODE_CERT
endif

.PHONY: ota_mqtt_sample resource_mqtt_sample data_template_sample asr_data_template_sample gateway_sample  mqtt_sample dynreg_dev_sample wifi_config_sample gateway_sim_sample subdev_sim_sample file_mqtt_sample kgmusic_data_template_sample location_sample

all: ota_mqtt_sample resource_mqtt_sample data_template_sample	asr_data_template_sample gateway_sample	mqtt_sample dynreg_dev_sample wifi_config_sample gateway_sim_sample subdev_sim_sample file_mqtt_sample kgmusic_data_template_sample location_sample


ifneq (,$(filter -DOTA_COMM_ENABLED,$(CFLAGS)))
ota_mqtt_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/ota/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DRESOURCE_UPDATE_ENABLED,$(CFLAGS)))
resource_mqtt_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/resource/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DFILE_MANAGE_ENABLED, $(CFLAGS)))
file_mqtt_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/file_manage/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DASR_ENABLED,$(CFLAGS)))
asr_data_template_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/asr/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DDATA_TEMPLATE_ENABLED,$(CFLAGS)))
data_template_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/data_template/$@.c $(LDFLAGS) -o $@
	
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/scenarized/light_$@.c $(LDFLAGS) -o light_$@
	
	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin && \
	mv light_$@ $(FINAL_DIR)/bin 
endif

ifneq (,$(filter -DMQTT_COMM_ENABLED,$(CFLAGS)))
mqtt_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/mqtt/$@.c $(LDFLAGS) -o $@
	
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/raw_data/*.c $(LDFLAGS) -o raw_data_$@
	
	$(TOP_Q) \
	mv raw_data_$@ $(FINAL_DIR)/bin && \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DGATEWAY_ENABLED,$(CFLAGS)))
gateway_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/gateway/*.c $(LDFLAGS) -o $@
	
	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
gateway_sim_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/scenarized/gateway_sim_sample.c $(LDFLAGS) -o $@
	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
subdev_sim_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/scenarized/subdev_sim_sample.c $(LDFLAGS) -o $@
	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DDEV_DYN_REG_ENABLED,$(CFLAGS)))
dynreg_dev_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/dynreg_dev/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DWIFI_CONFIG_ENABLED,$(CFLAGS)))
wifi_config_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/wifi_config/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (,$(filter -DKGMUSIC_ENABLED,$(CFLAGS)))
kgmusic_data_template_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/kgmusic/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

ifneq (, $(filter -DLOCATION_ENABLED, $(CFLAGS)))
location_sample:
	$(TOP_Q) \
	$(PLATFORM_CC) $(CFLAGS) $(SAMPLE_DIR)/location/$@.c $(LDFLAGS) -o $@

	$(TOP_Q) \
	mv $@ $(FINAL_DIR)/bin
endif

clean:
	rm -rf $(FINAL_DIR)/bin/*

