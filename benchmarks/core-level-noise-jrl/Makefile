KOKKOS_PATH = ${HOME}/kokkos/kokkos
SRC = $(wildcard *.cpp)
KOKKOS_DEVICES=Cuda
KOKKOS_CUDA_OPTIONS=enable_lambda
KOKKOS_ARCH = Power9,Volta70

default: build
	echo "Start Build"

ifneq (,$(findstring Cuda,$(KOKKOS_DEVICES)))
#OMPI_CXX= ${KOKKOS_PATH}/bin/nvcc_wrapper
NVCC_WRAPPER_DEFAULT_COMPILER = mpic++
CXX = ${KOKKOS_PATH}/bin/nvcc_wrapper
#CXX = mpic++
EXE = test.cuda
else
CXX = g++
EXE = test.host
endif

CXXFLAGS = -O3 -g 
LINK = ${CXX}
LINKFLAGS =  

DEPFLAGS = -M

OBJ = $(SRC:.cpp=.o)
LIB =

include $(KOKKOS_PATH)/Makefile.kokkos

build: $(EXE)

$(EXE): $(OBJ) $(KOKKOS_LINK_DEPENDS)
	$(LINK) $(KOKKOS_LDFLAGS) $(LINKFLAGS) $(EXTRA_PATH) $(OBJ) $(KOKKOS_LIBS) $(LIB) -o $(EXE)

clean: kokkos-clean 
	rm -f *.o *.cuda *.host

# Compilation rules

%.o:%.cpp $(KOKKOS_CPP_DEPENDS)
	$(CXX) $(KOKKOS_CPPFLAGS) $(KOKKOS_CXXFLAGS) $(CXXFLAGS) $(EXTRA_INC) -c $<
