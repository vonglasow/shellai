# ShellAI

## Requirements

- https://ollama.com
- httpx==0.26.0
- pydantic==2.5.3

## Installation

### MacOS

```sh
brew install ollama
ollama serve
ollama pull openhermes2.5-mistral

git clone git@github.com:vonglasow/shellai.git
```

You must have a `ollama serve` running somewhere
Add the path of shellai into your $PATH

## Usage

```sh
$ shellai -h
usage: shellai [-h] [--shell] [-c] [strings ...]

Process user input and command-line arguments.

positional arguments:
  strings     Additional strings to process

options:
  -h, --help  show this help message and exit
  --shell     Include shell information in the message context
  -c          Use CODE_ROLE format
```

```sh
$ shellai --shell "Give me a command to found all yaml files and delete it"
find . -name "*.yaml" -type f -exec rm {} \;
```

```sh
$ docker logs quant-ux-mongo | shellai "analyze error and warning and give me advice to fix it"
To fix the warnings and errors related to WiredTiger, you can follow these steps:

1. Upgrade MongoDB to the latest version as these warnings and errors may have been resolved in newer versions. You can check the official MongoDB release notes for details on the specific changes that address these issues.

2. Review the configuration settings of your MongoDB instance, specifically focusing on the WiredTiger cache parameters. Ensure that the cache settings are optimized for your workload and environment. Some key cache settings to consider include:
   - `wt.cache_size_mb`: Adjust the size of the WiredTiger global cache as needed. The default value is 25% of your available RAM, but you may need to increase or decrease this based on your specific requirements.
   - `wt.session_max_memory_operations` and `wt.engine_cache_essions_max`: These settings control the amount of memory used by WiredTiger for session caching. Adjust these values as needed to balance performance and resource usage.

3. Monitor your MongoDB instance's performance regularly, using tools like MongooseMetr or MongoDB Cloud Manager, to ensure that it is operating within expected parameters. If you continue to encounter issues, consider optimizing your workload by redesigning queries, indexes, or data models.

4. If the warnings and errors persist after attempting these steps, consult the official MongoDB documentation and support resources for further assistance. They can provide more specific guidance based on your environment and MongoDB version.
```

```sh
$ shellai -c "write a fibonacci function in python"
def fibonacci(n):
    if n <= 0:
        return []
    elif n == 1:
        return [0]
    elif n == 2:
        return [0, 1]
    else:
        sequence = [0, 1]
        while len(sequence) < n:
            sequence.append(sequence[-1] + sequence[-2])
        return sequence
```

```sh
$ cat CVE-2021-4034.py | shellai "Analyze and explain this code"
This code is a Python script that exploits the CVE-2021-4034 vulnerability in Python. It was originally written by Joe Ammond, who used it as an experiment to see if he could get it to work in Python while also playing around with ctypes.

The code starts by importing necessary libraries and defining variables. The `base64` library is imported to decode the payload, while the `os` library is needed for certain file operations. The `sys` library is used to handle system-level interactions, and the `ctypes` library is used to call the `execve()` function directly.

The code then decodes a base64 encoded ELF shared object payload from a previous command (in this case, using msfvenom). This payload is created with the PrependSetuid=true flag so that it can run as root instead of just the user.

An environment list is set to configure the call to `execve()`. The code also finds the C library, loads the shared library from the payload, creates a temporary file for exploitation, and makes necessary directories.

The code ends with calling the `execve()` function using the C library found earlier, passing in NULL arguments as required by `execve()`.
```

## Config

### Config example

```sh
touch ~/.config/shellai/config.json
```

```sh
{
    "OLLAMA_MODEL": "mistral",
    "TIMEOUT_SECONDS": 1,
    "BUFFER_SIZE": 500
}
```

### Default config

```sh
{
    "OLLAMA_BASE_URL": "http://localhost:11434",
    "OLLAMA_MODEL": "openhermes2.5-mistral",
    "TIMEOUT_SECONDS": 60,
    "BUFFER_SIZE": 500
}
```

## Thanks

Heavily inspired by [shell_gpt](https://github.com/TheR1D/shell_gpt)
