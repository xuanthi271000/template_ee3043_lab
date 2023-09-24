#*******************************************************************************
# Creator:        Hai Cao Xuan (cxhai.sdh221@hcmut.edu.vn)
# Changer:        Thi Pham Xuan (thi.phambk2018@hcmut.edu.vn)
# Description:    makefile
#*******************************************************************************
# Copyright (c) 2022 Hai Cao Xuan
#*******************************************************************************

FILES := $(shell cat filelist)

VER = verilator
VER_COV = verilator_coverage

### LINT FLAGS
LINT_FLAGS = -Wall --lint-only -sv

### VERILATE FLAGS
# Generate C++ in executable form
VER_FLAGS += -sv -cc
# Optimize
VER_FLAGS += -x-initial unique
VER_FLAGS += -x-assign unique
VER_FLAGS += -O3 -CFLAGS -DCOVERAGE
# Warn abount lint issues; may not want this on less solid designs
VER_FLAGS += -Wall
# Enable assertion
VER_FLAGS += --assert
# Make waveforms
VER_FLAGS += --trace-fst
VER_FLAGS += --build -j

# Input files for Verilator
VER_INPUT = $(FILES) top.sv --top-module top

.PHONY: help
help:
	@echo -e "make [option]"
	@echo -e "\tlint    run lint checks for syntax and violations"
	@echo -e "\tbuild   verilate and build modules"
	@echo -e "\tsim     run simulation"
	@echo -e "\twave    open waveforms"
	@echo -e "\tclean   clean the working folder"
	@echo -e "\thelp    print this help"

.PHONY: lint
lint:
	@echo "------------------> LINT CHECK <------------------"
	@$(VER) $(LINT_FLAGS) $(FILES)

.PHONY: build
build: ./obj_dir/Vtop
./obj_dir/Vtop: $(FILES) top.sv tb_top.cpp
	@echo "-------------------> BUILD <----------------------"
	@$(VER) $(VER_FLAGS) $(VER_INPUT) +define+$(define) --exe tb_top.cpp
                                     
.PHONY: sim                          
sim: ./obj_dir/Vtop                  
	@echo "-----------------> SIMULATING <-------------------"
	@./obj_dir/Vtop

.PHONY: wave
wave: wave.fst
	@echo "-----------------> WAVEFORMS <--------------------"
	@gtkwave wave.fst wave.gtkw

.PHONY: clean
clean:
	@rm -rf obj_dir
	@rm -f *.fst
	@echo "-----------------> ALL CLEAN <--------------------"
