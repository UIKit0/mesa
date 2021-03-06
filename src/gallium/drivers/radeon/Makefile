
TOP = ../../../..
include $(TOP)/configs/current

include Makefile.sources

LIBNAME = radeon

LIBRARY_INCLUDES = -I$(TOP)/include

TBLGEN = $(LLVM_BINDIR)/llvm-tblgen

CXXFLAGS+= $(LLVM_CXXFLAGS)

include ../../Makefile.template

CXXFLAGS := $(filter-out -DDEBUG, $(CXXFLAGS))

tablegen = $(TBLGEN) -I $(LLVM_INCLUDEDIR) $1 $2 -o $3

HAVE_LLVM_INTRINSICS = $(shell grep IntrinsicsR600.td $(LLVM_INCLUDEDIR)/llvm/Intrinsics.td)

gen: $(GENERATED_SOURCES)

SIRegisterInfo.td: SIGenRegisterInfo.pl
	$(PERL) $^ > $@

SIRegisterGetHWRegNum.inc: SIGenRegisterInfo.pl
	$(PERL) $^ $@ > /dev/null

R600Intrinsics.td: R600IntrinsicsNoOpenCL.td R600IntrinsicsOpenCL.td
ifeq ($(HAVE_LLVM_INTRINSICS),)
	cp R600IntrinsicsNoOpenCL.td R600Intrinsics.td
else
	cp R600IntrinsicsOpenCL.td R600Intrinsics.td
endif

R600RegisterInfo.td: R600GenRegisterInfo.pl
	$(PERL) $^ > $@

AMDGPUGenRegisterInfo.inc: *.td
	$(call tablegen, -gen-register-info, AMDGPU.td, $@)

AMDGPUGenInstrInfo.inc: *.td
	$(call tablegen, -gen-instr-info, AMDGPU.td, $@)

AMDGPUGenAsmWriter.inc: *.td
	$(call tablegen, -gen-asm-writer, AMDGPU.td, $@)

AMDGPUGenDAGISel.inc: *.td
	$(call tablegen, -gen-dag-isel, AMDGPU.td, $@)

AMDGPUGenCallingConv.inc: *.td
	$(call tablegen, -gen-callingconv, AMDGPU.td, $@)

AMDGPUGenSubtargetInfo.inc: *.td
	$(call tablegen, -gen-subtarget, AMDGPU.td, $@)

AMDGPUGenEDInfo.inc: *.td
	$(call tablegen, -gen-enhanced-disassembly-info, AMDGPU.td, $@)

AMDGPUGenIntrinsics.inc: *.td
	$(call tablegen, -gen-tgt-intrinsic, AMDGPU.td, $@)

AMDGPUGenCodeEmitter.inc: *.td
	$(call tablegen, -gen-emitter, AMDGPU.td, $@)

AMDGPUGenDFAPacketizer.inc: *.td
	$(call tablegen, -gen-dfa-packetizer, AMDGPU.td, $@)

LOADER_LIBS=$(shell llvm-config --libs bitreader asmparser)
loader: loader.o libradeon.a
	gcc -o loader -L/usr/local/lib $(LDFLAGS) loader.o libradeon.a $(LLVM_LIBS) $(LOADER_LIBS) -lpthread -ldl -lstdc++ -lm
