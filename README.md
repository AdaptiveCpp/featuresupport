# hipSYCL feature support

This page summarizes the state of SYCL feature support in the current `develop` branch of hipSYCL. Features that are supported are listed with a link to the pull request where they have been merged.

## SYCL 1.2.1 features
(This list is incomplete and only contains features that are known to be problematic)

| Feature | Supported (PR link) | Caveats | Comments |
| --- | --- | --- | --- |
| Images | :x: | --- | --- |
| OpenCL interop | :x: | --- | --- |
| Hierarchical parallelism | :heavy_check_mark: | HIP/CUDA: Does not limit execution in work group scope to one thread for performance reasons | |

## SYCL 2020 features

| Feature | Supported (PR link) | Caveats | Comments |
| --- | --- | --- | --- |
| Accessor simplifications | :heavy_check_mark: (partial) ([PR](https://github.com/illuhad/hipSYCL/pull/490)) | [6] |  |
| USM: Memory management functions | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/308))| [1] | |
| USM: Queue shortcuts | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/323)) | | |
| USM: Prefetch | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/323)) | [2] | |
| USM: mem_advise | :x: |  | Implementation requires host tasks since backends do not provide async mem advise |
| USM: memcpy | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/323)) | | |
| USM: memset/fill | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/323)) | | |
| host tasks | :x: |  |  |
| Optional lambda naming | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/281)) | | |
| Subgroups | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/282)) | | On CPU, subgroup size is always 1 |
| In-order queues | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/320)) | | |
| Explicit dependencies (`depends_on()`) | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/320)) | | |
| Backend interop API | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/327)) | [3] | |
| Reductions | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/374)) | [4] | |
| Group algorithms | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/391)) | [5] | |
| New device selector API | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/531)) | | |
| Aspect API | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/530)) | | |
| Deduction guides | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/388)) | | |
| `atomic_ref` | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/584)) | | |
| `marray` | :x: | | |
| New `SYCL/sycl.hpp` header | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/216)) | | |
| C++17 by default | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/206)) | | |
| Builtin changes: `ctz()`, `clz()` | :x: | | |
| Remove `*_class` types | :x: | | |
| `const` return type for read accessor `operator[]` | :x: | | |
| Remove buffer API for `unique_ptr` | :x: | | |
| Replace `program` class with `module` | :x: | | |
| Add `kernel_handler` | :x: | | |
| explicit `queue`, `context` constructors | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/328)) | | |
| Only require C++ trivially copyable for shared data | :heavy_check_mark: | | Has always worked thanks to CUDA/HIP toolchain |
| Update group class with new types/member functions | :x: | | |
| Remove `nd_item::barrier()` | :x: | | |
| Replace `mem_fence` with `atomic_fence` | :x: | | |
| Add `vec::operator[]`,unary `+,-`, `static constexpr get_size()/get_count()` | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/415)) | | |
| buffer, local accessor are C++ `ContiguousContainer` | :x: | | |
| Replace `image` with `sampled_image`, `unsampled_image` | :x: | | |
| All accessors are placeholders | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/490)) | | |
| Use single exception type derived from `std::exception` | :x: | | |
| Default asynchronous handler should terminate program  | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/289)) | | |
| Kernel invocation APIs take const reference to kernels, kernels must be immutable | :x: | | |
| Queue constructor accepting both `device` and `context` | :x: | | |
| Simplified `parallel_for` API | :x: | | |
| Clarified names for device specific info queries | :x: | | |
| Address space changes, generic address spaces | :x: | | Partially, we have always had generic address spaces because of CUDA/HIP |
| Updated `multi_ptr` interface | :x: | | |
| Remove OpenCL types, `cl_int` etc | :heavy_check_mark: | | hipSYCL has stopped supporting them a long time ago |




* [1] HIP/ROCm implements unified memory using slow device accessible host memory. This means that hipSYCL's call to `hipMallocManaged` cannot produce efficient shared allocations.
* [2] HIP/ROCm does not provide the required functionality, so hipSYCL cannot expose it. Prefetch calls are ignored at the moment.
* [3] The interop types that backends expose is limited. Native queues can only be obtained using an `interop_handle` because a queue in hipSYCL does not relate to any specific backend object.
* [4] Only scalar reductions are supported. Note that the reduction interface is expected to change slightly with the release of SYCL 2020 final.
* [5] Note that the interface of group algorithms is expected to change slightly with the release of SYCL 2020 final.
* [6] Constructing read-only accessor using `accessor<const T>` from non-const `buffer<T>` is not yet supported.
