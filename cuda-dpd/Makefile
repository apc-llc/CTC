CXXFLAGS += -g -O2 -std=c++11 -I/usr/local/cuda-5.5/include/ -fPIC

#these setting are for my gk104, for k20x pls set the right flags
NVCCFLAGS = -arch=compute_30  -Xcompiler -fpic -code=sm_30  -O4
#NVCCFLAGS += -Xptxas -v,-abi=yes
NVCCFLAGS += -Xptxas -dlcm=cg
NVCCFLAGS += --maxrregcount 30
NVCCFLAGS += --use_fast_math


ifeq "$(profile)" "1"
NVCCFLAGS += -lineinfo -D_PROFILE_ -g
CXXFLAGS += -D_PROFILE_
endif

ifneq "$(debug)" "1"
NVCCFLAGS += -DNDEBUG
else
NVCCFLAGS += -g
endif

NVCC = nvcc

test: main.cpp cuda-dpd.o profiler-dpd.o cell-lists.o
	$(CXX) $(CXXFLAGS) main.cpp cuda-dpd.o profiler-dpd.o cell-lists.o -L/usr/local/cuda-5.5/lib64/ -lcudart -lcurand -o test

libcuda-dpd.so: cuda-dpd.o cell-lists.o profiler-dpd.o
	$(CXX) $(CXXFLAGS) -shared $^ -o libcuda-dpd.so -L/usr/local/cuda-5.5/lib64/ -lcudart -lcurand

cuda-dpd.o: cuda-dpd.cu cuda-dpd.h
	$(NVCC) $(NVCCFLAGS) -c cuda-dpd.cu

profiler-dpd.o:	profiler-dpd.cpp profiler-dpd.h
	$(CXX) $(CXXFLAGS) -c profiler-dpd.cpp

cell-lists.o: cell-lists.cu cell-lists.h
	$(NVCC) $(NVCCFLAGS) -c cell-lists.cu

clean:
	rm -f test libcuda-dpd.so *.o 

.PHONY = clean
