[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/horw-esp-mcp-badge.png)](https://mseep.ai/app/horw-esp-mcp)

### Goal
The goal of this MCP is to:
- Consolidate ESP-IDF and related project commands in one place.
- Simplify getting started using only LLM communication.

### How to contribute to the project

Simply find a command that is missing from this MCP and create a PR for it!

If you want someone to help you with this implementation, just open an issue.


### Notice
This project is currently a **Proof of Concept (PoC)** for an MCP server tailored for ESP-IDF workflows.

**Current Capabilities:**

**Core Features (30 tools available):**

**Project Management:**
*   `create_esp_project`: Create a new ESP-IDF project.
*   `setup_project_esp_target`: Set target chip for ESP-IDF projects (esp32, esp32c3, esp32s3, etc.).
*   `get_project_info`: Get detailed information about an ESP-IDF project.
*   `list_components`: List all components in an ESP-IDF project.

**Build & Flash:**
*   `build_esp_project`: Build ESP-IDF projects with incremental build support.
*   `clean_esp_project`: Clean build files from an ESP-IDF project.
*   `flash_esp_project`: Flash built firmware to connected ESP devices.
*   `erase_flash_esp`: Erase flash memory on ESP device.
*   `flash_and_monitor_esp`: Flash firmware and immediately monitor serial output.

**Device Operations:**
*   `list_esp_serial_ports`: List available serial ports for ESP devices.
*   `monitor_esp`: Monitor serial output from ESP device.

**Configuration:**
*   `menuconfig_esp`: Run menuconfig to configure ESP-IDF project.
*   `get_project_config`: Get project configuration information (sdkconfig).
*   `set_esp_partition`: Set partition table for ESP-IDF project.
*   `get_esp_idf_version`: Get ESP-IDF version information.
*   `check_esp_idf_env`: Check ESP-IDF environment status and configuration.
*   `run_esp_idf_install`: Run install.sh script in ESP-IDF directory.

**Debugging:**
*   `gdb_attach`: Attach GDB debugger to ESP device.
*   `get_core_dump`: Get core dump information from ESP device.

**Runtime Analysis:**
*   `get_heap_info`: Get heap memory information from ESP device.
*   `get_task_stats`: Get FreeRTOS task statistics from ESP device.

**Testing:**
*   `run_pytest`: Run pytest tests with pytest-embedded support for ESP-IDF projects.

**File Operations:**
*   `read_file`: Read contents of a file in the project.
*   `write_file`: Write content to a file in the project.
*   `list_files`: List files and directories in a project path.

**Analysis Tools:**
*   `parse_build_log`: Parse and analyze build log with structured output for AI analysis.
*   `analyze_memory_map`: Analyze memory usage from .map file.
*   `compare_sdkconfig`: Compare two sdkconfig files and output structured differences.
*   `analyze_dependencies`: Analyze component dependencies from CMakeLists.txt files.
*   `format_device_log`: Parse and format device serial logs with structured output.

**Additional Features:**
*   Flexible ESP-IDF path management: supports per-project ESP-IDF versions via `idf_path` parameter.
*   Dynamic ESP-IDF path detection: automatically reads IDF_PATH from MCP configuration.
*   SDK config management: supports custom `sdkconfig_defaults` files for build configuration (multiple files can be specified separated by semicolons).
*   Build time tracking for performance monitoring.
*   Optional port specification for flashing operations.
*   Windows Git Bash path compatibility: automatic path conversion for cross-platform support.

**Recent Bug Fixes & Improvements:**
*   Fixed event loop nesting errors by implementing synchronous server communication
*   Fixed JSON-RPC protocol errors with proper empty line handling
*   Improved environment variable handling with better error messages
*   Fixed subprocess encoding issues (UTF-8 with error replacement)
*   Added Git Bash path conversion for Windows (E:/path → /e/path)
*   Implemented dynamic ESP-IDF path detection from MCP configuration
*   Added comprehensive error handling and logging

**Vision & Future Work:**
The long-term vision is to expand this MCP into a comprehensive toolkit for interacting with embedded devices, potentially integrating with home assistant platforms, and streamlining documentation access for ESP-IDF and related technologies.

We envision features such as:
*   Broader ESP-IDF command support (e.g., `monitor`, `menuconfig` interaction if feasible).
*   Device management and information retrieval.
*   Integration with other embedded development tools and platforms.

Your ideas and contributions are welcome! Please feel free to discuss them by opening an issue.


### Install

First, clone this MCP repository:

```bash
git clone git@github.com:horw/esp-mcp.git
```

Then, configure it in your chatbot.

The JSON snippet below is an example of how you might configure this `esp-mcp` server within a chatbot or an agent system that supports the Model Context Protocol (MCP). The exact configuration steps and format may vary depending on the specific chatbot system you are using. Refer to your chatbot's documentation for details on how to integrate MCP servers.

```json
{
    "mcpServers": {
        "esp-run": { // "esp-run" is an arbitrary name you can assign to this server configuration.
            "command": "<path_to_uv_or_python_executable>",
            "args": [
                "--directory",
                "<path_to_cloned_esp-mcp_repository>", // e.g., /path/to/your/cloned/esp-mcp
                "run",
                "main.py" // If using python directly, this might be just "main.py" and `command` would be your python interpreter
            ],
            "env": {
                "IDF_PATH": "<path_to_your_esp-idf_directory>" // e.g., ~/esp/esp-idf or C:\\Espressif\\frameworks\\esp-idf
            }
        }
    }
}
```

A few notes on the configuration:

*   **`command`**: This should be the full path to your `uv` executable if you are using it, or your Python interpreter (e.g., `/usr/bin/python3` or `C:\\Python39\\python.exe`) if you plan to run `main.py` directly.
*   **`args`**:
    *   The first argument to `--directory` should be the absolute path to where you cloned the `esp-mcp` repository.
    *   If you're using `uv`, the arguments `run main.py` are appropriate. If you're using Python directly, you might only need `main.py` in the `args` list, and ensure your `command` points to the Python executable.
*   **`IDF_PATH`**: (Optional) This environment variable can point to the root directory of your ESP-IDF installation. ESP-IDF is Espressif's official IoT Development Framework. If you haven't installed it, please refer to the [official ESP-IDF documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html) for installation instructions. **Note**: All tools support an `idf_path` parameter that can be manually specified when calling the tool, allowing you to use different ESP-IDF versions for different projects without setting the environment variable. If `idf_path` is not provided, the tool will use the `IDF_PATH` environment variable if available.

### Usage

Once the `esp-mcp` server is configured and running, your LLM or chatbot can interact with it using the tools defined in this MCP. For example, you could ask your chatbot to:

*   "Install ESP-IDF dependencies for the ESP-IDF installation at `/path/to/esp-idf`."
*   "Set the target chip to esp32s3 for the project in `/path/to/my/esp-project`."
*   "Build the project located at `/path/to/my/esp-project` using the `esp-mcp`."
*   "Build the project with custom sdkconfig defaults: `sdkconfig.defaults;sdkconfig.ci.release`."
*   "Run pytest tests for the project at `/path/to/my/esp-project` targeting esp32c3."
*   "Flash the firmware to my connected ESP32 device for the project in `my_app`."

The MCP server will then execute the corresponding ESP-IDF commands (like `idf.py build`, `idf.py set-target`, `idf.py flash`, `pytest`) based on the tools implemented in `main.py`.

The `result.gif` below shows an example interaction:

![Result](./result.gif)


### Examples


1. Build and Flash
<img src="./examples/build-flash.png">

### Star History

[![Star History Chart](https://api.star-history.com/svg?repos=horw/esp-mcp&type=Date)](https://star-history.com/#horw/esp-mcp&Date)





