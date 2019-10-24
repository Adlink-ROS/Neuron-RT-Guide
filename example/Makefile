XENO ?= /usr/xenomai
XENOCONFIG=$(shell PATH=$(XENO):$(XENO)/bin:$(PATH) which xeno-config 2>/dev/null)

CC=$(shell $(XENOCONFIG) --cc)

CFLAGS=$(shell $(XENOCONFIG) --skin=native --cflags)

#LDLIBS=$(shell $(XENOCONFIG) --skin=native --ldflags)
LDLIBS=$(shell $(XENOCONFIG) --skin=rtdm --ldflags)

LDFLAGS+=-Xlinker -rpath -Xlinker $(shell $(XENOCONFIG) --libdir)

all: example
