CXX = $(shell yosys-config --cxx)
CXXFLAGS = $(shell yosys-config --cxxflags)
LDFLAGS = $(shell yosys-config --ldflags)
LDLIBS = $(shell yosys-config --ldlibs)
PLUGINS_DIR = $(shell yosys-config --datdir)/plugins

OBJS = vhdl_parser.tab.o vhdl_lexer.o vhdl_frontend.o preproc.o

vhdl.so: vhdl_parser.tab.o vhdl_lexer.o vhdl_frontend.o preproc.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) -shared -o $@ $^ $(LDLIBS)
	$(MAKE) test

vhdl_parser.tab.cc: vhdl_parser.y def.h
	bison -d -r all -b vhdl_parser $<
	mv vhdl_parser.tab.c vhdl_parser.tab.cc

vhdl_parser.tab.h: vhdl_parser.tab.cc

vhdl_lexer.cc: vhdl_lexer.l def.h vhdl_parser.tab.h
	flex -d -i -o $@ $<

.PHONY: test
test:
	cd test; ./runtests

.PHONY: install
install: vhdl.so
	mkdir -p $(PLUGINS_DIR)
	cp vhdl.so $(PLUGINS_DIR)/vhdl.so

clean:
	rm -f *.d *.o vhdl.so vhdl_lexer.cc vhdl_parser.output vhdl_parser.tab.cc vhdl_parser.tab.h

