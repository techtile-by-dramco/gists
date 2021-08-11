## Download latest GCC version from https://developer.amd.com/amd-aocl/
## Example: aocl-linux-gcc-3.0_6_1_amd64.deb 


apt update --fix-missing
apt install -y build-essential

## install AOCL
apt install -y /tmp/aocl-linux-gcc-3.0_6_1_amd64.deb 

## install NumPy
cd /opt
git clone https://github.com/numpy/numpy.git
cd ./numpy

## Link to libs from AOCL

cat <<EOL >> site.cfg

[blis]                                                                              
libraries = blis-mt                                        
library_dirs = /opt/AMD/aocl/aocl-linux-gcc-3.0-6/lib
include_dirs = /opt/AMD/aocl/aocl-linux-gcc-3.0-6/include
runtime_library_dirs = /opt/AMD/aocl/aocl-linux-gcc-3.0-6/lib

[flame]                                                            
libraries = flame                                  
library_dirs = /opt/AMD/aocl/aocl-linux-gcc-3.0-6/lib
runtime_library_dirs = /opt/AMD/aocl/aocl-linux-gcc-3.0-6/lib
EOL


## Build from source
NPY_BLAS_ORDER=blis NPY_LAPACK_ORDER=flame pip install .
