# PrettyPrint

A versatile command-line tool to print code files with text only formatting, filtering options, and project structure visualization.

![pp](https://github.com/user-attachments/assets/acd80b13-4d9a-4c9c-9a42-6f89d6fdcd30)

## Overview

PrettyPrint is a Bash script that helps you view, share, and document your code by printing files with proper formatting. It offers powerful filtering capabilities, directory traversal, and project structure visualization, making it ideal for code reviews, documentation, or copying code to the clipboard.

## Features

- **Multiple Input Targets**: Process individual files, directories, or combinations
- **Recursive Processing**: Automatically traverse subdirectories
- **Path Exclusions**: Skip specific files or directories
- **File Type Filtering**: Include or exclude files based on extensions
- **Hidden Files Control**: Option to include or exclude hidden files
- **Binary Files Handling**: Automatically skip binary files (configurable)
- **File Structure Display**: Visualize your project structure in tree format
- **Markdown-Friendly Output**: Format code for easy pasting into documentation
- **Line Count Limiting**: Skip files exceeding a specified line count

## Installation

1. Download the script:
   ```bash
   curl -o pretty_print.sh https://raw.githubusercontent.com/yourusername/prettyprint/main/pretty_print.sh
   ```

2. Make it executable:
   ```bash
   chmod +x pretty_print.sh
   ```

3. Optional: Move to a directory in your PATH for easy access:
   ```bash
   sudo mv pretty_print.sh /usr/local/bin/prettyprint
   ```

## Usage

Basic usage:

```bash
./pretty_print.sh [OPTIONS] PATH [PATH...]
```

If no path is specified, the current directory will be used.

### Examples

Print all files in the current directory:
```bash
./pretty_print.sh .
```

Print specific files:
```bash
./pretty_print.sh app.py utils/helpers.js
```

Print Python and JavaScript files only:
```bash
./pretty_print.sh . --whitelist=py,js
```

Exclude specific directories:
```bash
./pretty_print.sh . --exclude=node_modules,build,__pycache__
```

Exclude specific file types:
```bash
./pretty_print.sh . --blacklist=json,svg,png
```

Exclude hidden files and directories:
```bash
./pretty_print.sh . --no-hidden
```

Print the full project structure:
```bash
./pretty_print.sh . --print-full-structure
```

Don't print the file structure at all:
```bash
./pretty_print.sh . --no-structure
```

Use UTF-8 characters in the structure tree:
```bash
./pretty_print.sh . --utf8
```

Copy output to clipboard (WSL example):
```bash
./pretty_print.sh . --whitelist=py,js | clip.exe
```

## Command Line Options

| Option | Description |
|--------|-------------|
| `--exclude=PATH1,PATH2,...` | Exclude specific paths (files or directories) |
| `--whitelist=EXT1,EXT2,...` | Only include files with specified extensions |
| `--blacklist=EXT1,EXT2,...` | Exclude files with specified extensions |
| `--no-hidden` | Exclude hidden files and directories |
| `--include-binary` | Include binary files (disabled by default) |
| `--max-lines=NUM` | Set maximum allowed lines (default: 1000) |
| `--print-full-structure` | Print the entire file structure |
| `--no-structure` | Don't print file structure at all |
| `--utf8` | Use UTF-8 characters in tree structure (default: ASCII) |
| `--debug` | Show debug information |
| `--help` | Display help message and exit |

## Output Format

The output consists of two main sections:

1. **File Structure** (if enabled):
   ```
   # File Structure

   +-- Project Structure
   |-- app.py
   |-- +controllers/
   |   |-- __init__.py
   |   |-- routes.py
   |   `-- models.py
   `-- requirements.txt

   ---
   ```

2. **File Contents**:

   # File Contents

   ==================
   Path: app.py
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello, World!'
   ```
   ==================


   ==================
   Path: controllers/routes.py
   ```python
   # Route definitions
   ...
   ```
   ==================

## Customizing Binary Files

The script has a built-in list of file extensions considered binary. You can customize this list by editing the `BINARY_EXTENSIONS` array in the script:

```bash
BINARY_EXTENSIONS=(
    "pdf" "png" "jpg" "jpeg" "gif" "bmp" "ico" "svg" "webp"
    "mp3" "mp4" "wav" "ogg" "flac" "avi" "mov" "mkv" "wmv"
    "zip" "tar" "gz" "bz2" "xz" "7z" "rar"
    "exe" "dll" "so" "dylib" "class" "pyc" "pyo" 
    "o" "obj" "a" "lib" "bin"
)
```

## Structure Visualization Options

PrettyPrint offers two structure display modes:

1. **Default Mode** - Shows only the structure of included files
2. **Full Structure Mode** - Shows all files and directories, including those not printed

The structure display can use either:
- ASCII characters (default, best for clipboard compatibility)
- UTF-8 characters with emojis (enabled with `--utf8`, better for terminal viewing)

## Troubleshooting

### No Files Displayed

If no files are displayed:

1. Check your filters (whitelist, blacklist, exclusions)
2. Verify file permissions
3. Run with `--debug` to see what's happening
4. Make sure target directories exist and are readable

### Structure Display Problems

If the structure display looks odd in your terminal or when copied:

1. Use the default ASCII mode for better compatibility
2. Try the `--utf8` option if your terminal supports it
3. Use `--no-structure` to disable the structure display

### Performance Issues

For large projects:

1. Be more specific with target directories
2. Use exclusions for large folders: `--exclude=node_modules,vendor,build`
3. Use whitelist to limit file types: `--whitelist=py,js,go`
