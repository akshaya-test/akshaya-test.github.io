# HEBench - Getting Started

To get a jump start on HEBench, users can benchmark one of the published reference backends. These backends have been designed to build and run with minimal configuration requirements.

- [Benchmarking a Reference Backend](#benchmarking-a-reference-backend)
- [Benchmarking a Custom Backend](#benchmarking-a-custom-backend)
- [Adding New Workloads](#adding-new-workloads)
- [References](#references)

## Benchmarking a Reference Backend

This guide will walk you through the steps of running the benchmark for the reference SEAL backend.

1. Pick the SEAL backend repository from the list of [published backends](hebench_published_backends.md), or visit direct link to [SEAL backend repository](https://github.com/hebench/backend-cpu-seal).

2. Check the [readme](https://github.com/hebench/backend-cpu-seal/blob/main/README.md) for requirements to build the backend.
   Make sure the machine where the build will occur meets such requirements.

3. Clone the repo to your local machine<b>*</b>.

   Assume the repo will be cloned cloned to the location contained in environment variable `$SEAL_BACKEND`.

   Note that repo URL may change. Obtain the correct URL from the GitHub repo itself.

```bash
cd $SEAL_BACKEND
git clone https://github.com/hebench/backend-cpu-seal.git
```

<b>*</b>_Users must have, at least, read access to the GitHub repository. Otherwise, cloning and/or building steps may fail._

4. Build with default settings.

   The following commands will create a build directory, pull down all the required dependencies, build all dependencies and the backend itself, and install the backend and Test Harness.

```bash
cd $SEAL_BACKEND/backend-cpu-seal
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=$SEAL_BACKEND/backend-cpu-seal/install -DCMAKE_BUILD_TYPE=Release ..
make -j
make install
```

5. Create a directory to hold the report of the benchmark we are about to run:

```bash
mkdir $HOME/backend-cpu-seal-report
```

6. Navigate to directory where the test harness was installed:

```bash
cd $SEAL_BACKEND/backend-cpu-seal/install/bin
```

7. Run the benchmark by executing test_harness while pointing it to the shared library containing the compiled SEAL reference backend.

   If these instructions have been followed correctly, the shared library for the SEAL reference backend should have been installed in `$SEAL_BACKEND/backend-cpu-seal/install/lib` .


```bash
./test_harness --backend_lib_path $SEAL_BACKEND/backend-cpu-seal/install/lib/libhebench_seal_backend.so --report_root_path $HOME/backend-cpu-seal-report
```

8. After the execution completes, the report will be saved to the specified location `$HOME/backend-cpu-seal-report`.

   The report is a collection of `CSV` files organized in directories that represent the benchmark parameters used for each workload benchmarked. Each report has a `report.csv` file and, if the corresponding benchmark completed successfully, a `summary.csv` file in the same location. View the `summary.csv` for the readable information about the benchmark, or `report.csv` if the benchmark failed for the cause of failure.

<br/>

Follow similar instructions to run the [clear text reference backend](https://github.com/hebench/backend-cpu-cleartext) or [PALISADE reference backend](https://github.com/hebench/backend-cpu-palisade), if desired.

## Benchmarking a Custom Backend

The power of HEBench is in the ability for users to create custom backends to benchmark their own implementations of the supported workloads, running on whichever framework or hardware they prefer.

Creating a custom backend requires creating a shared library that exposes the API Bridge functionality. Once this is accomplished, the steps to run the benchmark are similar to those listed above.

To create a custom backend, it is required to clone and build [HEBench Frontend repository](https://github.com/hebench/frontend). This will pull and build Test Harness, API Bridge, documentation, and the tools required to create a custom backend (technically, only [API Bridge](https://github.com/hebench/api-bridge) is required to create a backend, but frontend contains the tools to benchmark the backend). Once cloned, follow the tutorial [Backend Creation using C++](https://hebench.github.io/frontend/simple_cpp_example.html) in the HEBench documentation. Note that it requires knowledge of C++ to complete.

For more information about creating custom backends, read [Backend Overview](https://hebench.github.io/frontend/backend_overview.html) in the documentation.

## Adding New Workloads

If there is a need to benchmark a workload that is not officially supported by HEBench, there is a feature that allows extending the frontend to support other workloads. This is an advanced topic, but it is well [documented](https://hebench.github.io/frontend/frontend_overview.html). Follow the tutorial [Adding a New Workload to Test Harness](https://hebench.github.io/frontend/extend_test_harness.html) to extend the Test Harness.

## References
[Tutorial: Creating Custom Backend](https://hebench.github.io/frontend/simple_cpp_example.html)<br/>
[Tutorial: Adding a New Workload to Test Harness](https://hebench.github.io/frontend/extend_test_harness.html)<br/>
[HEBench Frontend](https://github.com/hebench/frontend)<br/>
[Published Backends](hebench_published_backends.md)<br/>
[HEBench Documentation](https://hebench.github.io/frontend/index.html)

<br/>

Back to [HEBench Home](https://hebench.github.io/).