SRC_NAME = iperf-3.15
SRC_URL = https://downloads.es.net/pub/iperf/$(SRC_NAME).tar.gz
THISDIR = $(shell pwd)

all: config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./configure \
		--prefix=/usr \
		--disable-profiling \
		--without-openssl \
		--disable-shared \
		--enable-static \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/src/iperf3 /usr/bin/iperf3
	$(ROMFSINST) -p +x $(THISDIR)/iperf3.sh /usr/bin/iperf3.sh

