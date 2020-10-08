# hipSYCL feature support

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
| Accessor simiplifications | :x: |  |  |
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
| Reductions | :x: | | |
| Group algorithms | :x: | | |
| New device selector API | :x: | | |
| Aspect API | :x: | | |
| Deduction guides | :x: | | |
| `atomic_ref` | :x: | | |
| `marray` | :x: | | |
| New `SYCL/sycl.hpp` header | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/216)) | | |
| C++17 by default | :heavy_check_mark: ([PR](https://github.com/illuhad/hipSYCL/pull/206))
| Builtin changes: `ctz()`, `clz()` | :x: | | |
| Remove `*_class` types | :x: | | |
| `const` return type for read accessor `operator[]` | :x: | | |
| Remove buffer API for `unique_ptr` | :x: | | |
| Replace `program` class with `module` | :x: | | |


* [1] HIP/ROCm implements unified memory using slow device accessible host memory. This means that hipSYCL's call to `hipMallocManaged` cannot produce efficient shared allocations.
* [2] HIP/ROCm does not provide the required functionality, so hipSYCL cannot expose it. Prefetch calls are ignored at the moment.
* [3] The interop types that backends expose is limited. Native queues can only be obtained using an `interop_handle` because a queue in hipSYCL does not relate to any specific backend object.
