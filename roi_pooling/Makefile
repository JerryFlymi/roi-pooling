TF_INC = `python -c "import tensorflow; print(tensorflow.sysconfig.get_include())"`
TF_LIB = `python -c "import tensorflow; print(tensorflow.sysconfig.get_lib())"`

CUDA_LIB = /usr/local/cuda/lib64/

all: clean build test

build: roi_pooling.so

roi_pooling.cu.o: roi_pooling.cu.cc
	nvcc -std=c++11 -c -o $@ $? -I $(TF_INC) -D GOOGLE_CUDA=1 -x cu -Xcompiler -fPIC -D _GLIBCXX_USE_CXX11_ABI=0

roi_pooling.so: roi_pooling.cc roi_pooling.cu.o
	g++ -std=c++11 -shared -o $@ $? -I $(TF_INC) -fPIC -lcudart -L$(CUDA_LIB) -D _GLIBCXX_USE_CXX11_ABI=0

test: build
	python roi_pooling_test.py

clean:
	rm -f *.o *.so *.pyc *.npy
