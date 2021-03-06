EXEDIR := run
OBJDIR := bin
SRCDIR := src
INCDIR := inc
MAKEDIR := bin
LIBFILE := $(OBJDIR)/libStatObj.a

CXX := g++
EXTRA_WARNINGS := -Wcast-align -Wcast-qual -Wdisabled-optimization -Wformat=2 -Wformat-nonliteral -Wformat-security -Wformat-y2k -Winit-self -Winvalid-pch -Wlong-long -Wmissing-format-attribute -Wmissing-include-dirs -Wmissing-noreturn -Wpacked -Wpointer-arith -Wredundant-decls -Wstack-protector -Wswitch-default -Wswitch-enum -Wundef -Wunused -Wvariadic-macros -Wwrite-strings -Wabi -Wctor-dtor-privacy -Wnon-virtual-dtor -Wsign-promo -Wsign-compare #-Wunsafe-loop-optimizations -Wfloat-equal -Wsign-conversion -Wunreachable-code
CXXFLAGS := -Wall -Wextra -pedantic -Werror -Wshadow -Woverloaded-virtual -Wold-style-cast $(EXTRA_WARNINGS) -O2 -I $(INCDIR) -m64
LD := g++
LDFLAGS := -pthread -m64
LDLIBS := 

EXECUTABLES := $(addprefix $(EXEDIR)/, $(addsuffix .exe, $(notdir $(basename $(wildcard $(SRCDIR)/*.cxx))))) 
OBJECTS := $(addprefix $(OBJDIR)/, $(addsuffix .o, $(notdir $(basename $(wildcard $(SRCDIR)/*.cpp)))))

FIND_DEPS = $(CXX) $(CXXFLAGS) -MM -MG -MF $@ $<
EXPAND_DEPS = perl -pi -e 's|$*.o|$(OBJDIR)/$*.o $(MAKEDIR)/$*.d|g' $@
GET_DEPS = $(FIND_DEPS) && $(EXPAND_DEPS)
COMPILE = $(CXX) $(CXXFLAGS) -o $@ -c $<
LINK = $(LD) $(LDFLAGS) -o $@ $^ $(LDLIBS)

vpath %.cpp $(SRCDIR)
vpath %.cxx $(SRCDIR)
vpath %.hpp $(INCDIR)
vpath %.o $(OBJDIR)
vpath %.exe $(EXEDIR)
vpath %.d $(MAKEDIR)

all: $(EXECUTABLES)

-include $(addsuffix .d,$(addprefix $(MAKEDIR)/,$(notdir $(basename $(wildcard $(SRCDIR)/*.cpp)))))
-include $(addsuffix .d,$(addprefix $(MAKEDIR)/,$(notdir $(basename $(wildcard $(SRCDIR)/*.cxx)))))

$(LIBFILE): $(OBJECTS)

$(MAKEDIR)/%.d: $(SRCDIR)/%.cpp
	$(GET_DEPS)

$(MAKEDIR)/%.d: $(SRCDIR)/%.cxx
	$(GET_DEPS)

$(OBJDIR)/%.o: $(SRCDIR)/%.cpp
	$(COMPILE)

$(OBJDIR)/%.o: $(SRCDIR)/%.cxx
	$(COMPILE)

$(OBJDIR)/%.a:
	ar rcsv $@ $^

$(EXEDIR)/%.exe: $(OBJDIR)/%.o $(LIBFILE)
	$(LINK)

.DELETE_ON_ERROR:
