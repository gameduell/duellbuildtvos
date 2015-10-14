
ifeq ("$(ARCHS)","normal")
	ARCHS = arm64 i386
endif

HAXE_BUILDS := $(ARCHS:%=build-haxe-%)

ifeq ("$(ACTION)","clean")
default: clean
else
default: $(HAXE_BUILDS)
endif

ifeq ("$(CONFIGURATION)","Debug")
   BUILD_STYLE := Debug
endif

default: debug_print

ifeq ("$(BUILD_STYLE)","Debug")
   DEBUG := -debug
   DEF_DEBUG := -Ddebug
   CONFIG := Debug
   CONFIG_FILE := debug
else
   DEBUG :=
   DEF_DEBUG :=
   CONFIG := Release
   CONFIG_FILE := release
endif

ifeq ("$(CLANG_ENABLE_OBJC_ARC)", "YES")
   HXCPP_CLANG := -DHXCPP_CLANG -DOBJC_ARC
else
   HXCPP_CLANG :=
endif

BUILD_HXML_SDK := ""


HAXE_OS := $(PLATFORM_NAME)
ifeq ("$(HAXE_OS)","appletvsimulator")
   HAXE_OS := appletvsim
endif

CONFIG := $(CONFIG)-$(HAXE_OS)

debug_print:
	@echo "Make $(HAXE_BUILDS)"

LIB_BASE := build/$(CONFIG)/lib::MAIN::$(DEBUG)
LIB_DEST := $(DEBUG)/libHaxeApplication.a

export HXCPP_COMPILE_THREADS := 2

build-haxe-i386:
	@echo "Haxe simulator build CONFIG : $(CONFIG)"
	haxe Build_$(CONFIG_FILE)_simulator.hxml
	cd build/$(CONFIG); haxelib run hxcpp Build.xml -tvos -Dsimulator -DHXCPP_CPP11 $(DEF_DEBUG) $(HXCPP_CLANG) ::foreach (PLATFORM.HXCPP_COMPILATION_ARGS)::::__current__:: ::end::
	cp $(LIB_BASE).appletvos.a ../lib/i386$(LIB_DEST)
	touch ../Classes/Main.mm
	touch ../Classes/DUELLAppDelegate.m

build-haxe-arm64:
	@echo "Haxe arm64 build CONFIG : $(CONFIG)"
	haxe Build_$(CONFIG_FILE)_arm64.hxml
	cd build/$(CONFIG); haxelib run hxcpp Build.xml -Dappletvos -DHXCPP_M64 -DHXCPP_ARM64 -DHXCPP_CPP11 $(DEF_DEBUG) $(HXCPP_CLANG) ::foreach (PLATFORM.HXCPP_COMPILATION_ARGS)::::__current__:: ::end::
	cp $(LIB_BASE).appletvos-64.a ../lib/arm64$(LIB_DEST)
	touch ../Classes/Main.mm
	touch ../Classes/DUELLAppDelegate.m

clean:
	rm -rf build
