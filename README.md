# eigen-clang-weirdness

This is a reproduction repo for odd behavior I've encountered in working with Eigen 3.3.3 and Clang 3.9.

It appears that when compiling with `clang-3.9` at `-O2` or above, passing a `Matrix3d` to a function 
as a const reference, copying it and returning an element of the copy can lead to garbage being returned 
if certain other operations are performed inside the function.

## Reproduction instructions
```sh
git clone https://github.com/avalenzu/eigen-clang-weirdness.git
cd eigen-clang-weirdness
mkdir build && cd build
cmake ..
make install
# The following highlights the odd behavior
./install/bin/failure_case
# The following shows one way to avoid the odd behavior
./install/bin/success_case_extra_copy
```
