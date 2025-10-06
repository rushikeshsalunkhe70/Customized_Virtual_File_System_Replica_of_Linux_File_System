# Marvellous CVFS - User Guide

This comprehensive user guide will help you understand and effectively use the Marvellous Custom Virtual File System (CVFS).

## Table of Contents

- [Getting Started](#getting-started)
- [Command Reference](#command-reference)
- [Step-by-Step Tutorials](#step-by-step-tutorials)
- [Common Use Cases](#common-use-cases)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Advanced Usage](#advanced-usage)

## Getting Started

### Prerequisites

- C++ compiler (g++, clang++, or MSVC)
- Basic understanding of command-line interfaces
- Familiarity with file system concepts (helpful but not required)

### Installation and Setup

1. **Compile the Program**
   ```bash
   g++ -o cvfs program724.cpp
   ```

2. **Run the Program**
   ```bash
   ./cvfs
   ```

3. **Verify Installation**
   You should see the welcome message:
   ```
   ---------------------------------------------------------
   --------- Marvellous CVFS Started Succesfully ------------
   -----------------------------------------------------------
   
   Marvellous CVFS >
   ```

### First Steps

1. **Get Help**
   ```bash
   Marvellous CVFS > help
   ```

2. **View Manual Pages**
   ```bash
   Marvellous CVFS > man creat
   ```

3. **Create Your First File**
   ```bash
   Marvellous CVFS > creat myfile.txt 3
   ```

## Command Reference

### Basic Commands

#### `help`
Displays all available commands and their descriptions.

**Usage:**
```bash
Marvellous CVFS > help
```

**Output:**
```
---------------------------------------------------------
----------- Command Manual of Marvellous CVFS -----------
---------------------------------------------------------
man : It is used to display the specific manual page of command
exit : It is used to terminate the shell of Marvellous CVFS
clear : It is used to clear the console of Marvellous CVFS
creat : It is used to create new regular file
unlink : It is used to delete existing file
stat : It is used to display statistical information about file
ls : It is used to list out all files from the directory
write : It is used to write the data into the file
read : It is used to read the data from the file
---------------------------------------------------------
```

#### `man <command>`
Shows detailed manual page for a specific command.

**Usage:**
```bash
Marvellous CVFS > man creat
Marvellous CVFS > man write
Marvellous CVFS > man read
```

**Example Output:**
```
Description : This command is used to create new regular file on our file system

Usage : creat File_name Permissions
File_name : The name of file that you want to create
Permissions : 
1 : Read 
2 : Write 
3 : Read + Write
```

#### `clear`
Clears the console screen.

**Usage:**
```bash
Marvellous CVFS > clear
```

#### `exit`
Terminates the CVFS shell and returns to the operating system.

**Usage:**
```bash
Marvellous CVFS > exit
```

**Output:**
```
Thank you for using Marvellous CVFS
Deallocating all resources...
```

### File Management Commands

#### `creat <filename> <permission>`
Creates a new regular file with specified permissions.

**Usage:**
```bash
Marvellous CVFS > creat <filename> <permission>
```

**Parameters:**
- `filename`: Name of the file to create (max 49 characters)
- `permission`: File access permissions
  - `1`: Read only
  - `2`: Write only
  - `3`: Read + Write

**Examples:**
```bash
Marvellous CVFS > creat document.txt 3
File is succesfully created with FD : 0

Marvellous CVFS > creat readonly.txt 1
File is succesfully created with FD : 1

Marvellous CVFS > creat writeonly.txt 2
File is succesfully created with FD : 2
```

**Error Messages:**
- `Error : Invalid parameters for the function` - Invalid filename or permission
- `Error : Unable to create file as there is no Inodes` - System at capacity
- `Error : Unable to create file as file is already existing` - File name conflict

#### `ls`
Lists all files in the system.

**Usage:**
```bash
Marvellous CVFS > ls
```

**Example Output:**
```
document.txt
readonly.txt
writeonly.txt
```

#### `stat <filename>`
Displays detailed statistical information about a file.

**Usage:**
```bash
Marvellous CVFS > stat <filename>
```

**Example:**
```bash
Marvellous CVFS > stat document.txt
------------ Statistical Information of file -----------
File name : document.txt
File size on Disk : 100
Actual File size : 0
Link count : 1
File permission : Read + Write
File type : Regular file
--------------------------------------------------------
```

**Error Messages:**
- `Error : Unable to display statistics as file is not present` - File doesn't exist
- `Error : Invalid parameters for the function` - Invalid filename

#### `unlink <filename>`
Deletes a file from the system.

**Usage:**
```bash
Marvellous CVFS > unlink <filename>
```

**Example:**
```bash
Marvellous CVFS > unlink document.txt
Unlink Opertaion is succesfully performed
```

**Error Messages:**
- `Error : Unable to do unlink activity as file is not present` - File doesn't exist
- `Error : Invalid parameters for the function` - Invalid filename

### Data Operations

#### `write <file_descriptor>`
Writes data to a file using its file descriptor.

**Usage:**
```bash
Marvellous CVFS > write <file_descriptor>
```

**Process:**
1. Enter the command with file descriptor
2. System prompts for data input
3. Type your data and press Enter
4. Data is written to the file

**Example:**
```bash
Marvellous CVFS > write 0
Please enter the data that you want to write into the file : 
Hello, World! This is my first file.
35 bytes gets succesfully written into the file
Data from file is : Hello, World! This is my first file.
```

**Error Messages:**
- `Error : Insufficient space in tha data block for the file` - File is full
- `Error : Unable to write as there is no write permission` - Read-only file
- `Error : Invalid parameters for the function` - Invalid file descriptor
- `Error : FD is invalid` - File not open or invalid descriptor

#### `read <file_descriptor> <size>`
Reads data from a file using its file descriptor.

**Usage:**
```bash
Marvellous CVFS > read <file_descriptor> <size>
```

**Parameters:**
- `file_descriptor`: File descriptor returned by `creat` command
- `size`: Number of bytes to read

**Example:**
```bash
Marvellous CVFS > read 0 20
Read operation is successfull
Data from file is : Hello, World! This is
```

**Error Messages:**
- `Error : Insufficient data in tha data block of the file` - Not enough data
- `Error : Unable to redad as there is no read permission` - Write-only file
- `Error : Invalid parameters for the function` - Invalid parameters
- `Error : FD is invalid` - File not open or invalid descriptor

## Step-by-Step Tutorials

### Tutorial 1: Basic File Operations

**Objective:** Create, write to, read from, and delete a file.

**Steps:**

1. **Start the system**
   ```bash
   ./cvfs
   ```

2. **Create a file**
   ```bash
   Marvellous CVFS > creat tutorial.txt 3
   File is succesfully created with FD : 0
   ```

3. **Write data to the file**
   ```bash
   Marvellous CVFS > write 0
   Please enter the data that you want to write into the file : 
   This is my tutorial file.
   25 bytes gets succesfully written into the file
   ```

4. **Read data from the file**
   ```bash
   Marvellous CVFS > read 0 25
   Read operation is successfull
   Data from file is : This is my tutorial file.
   ```

5. **Check file statistics**
   ```bash
   Marvellous CVFS > stat tutorial.txt
   ------------ Statistical Information of file -----------
   File name : tutorial.txt
   File size on Disk : 100
   Actual File size : 25
   Link count : 1
   File permission : Read + Write
   File type : Regular file
   --------------------------------------------------------
   ```

6. **List all files**
   ```bash
   Marvellous CVFS > ls
   tutorial.txt
   ```

7. **Delete the file**
   ```bash
   Marvellous CVFS > unlink tutorial.txt
   Unlink Opertaion is succesfully performed
   ```

8. **Verify deletion**
   ```bash
   Marvellous CVFS > ls
   (no output - file deleted)
   ```

9. **Exit the system**
   ```bash
   Marvellous CVFS > exit
   ```

### Tutorial 2: Working with Multiple Files

**Objective:** Create multiple files with different permissions and manage them.

**Steps:**

1. **Create multiple files**
   ```bash
   Marvellous CVFS > creat readme.txt 3
   File is succesfully created with FD : 0
   
   Marvellous CVFS > creat config.txt 1
   File is succesfully created with FD : 1
   
   Marvellous CVFS > creat temp.txt 2
   File is succesfully created with FD : 2
   ```

2. **Write to read-write file**
   ```bash
   Marvellous CVFS > write 0
   Please enter the data that you want to write into the file : 
   This is a readme file.
   22 bytes gets succesfully written into the file
   ```

3. **Try to write to read-only file (should fail)**
   ```bash
   Marvellous CVFS > write 1
   Please enter the data that you want to write into the file : 
   This should fail.
   Error : Unable to write as there is no write permission
   ```

4. **Write to write-only file**
   ```bash
   Marvellous CVFS > write 2
   Please enter the data that you want to write into the file : 
   This is temporary data.
   20 bytes gets succesfully written into the file
   ```

5. **Read from read-write file**
   ```bash
   Marvellous CVFS > read 0 22
   Read operation is successfull
   Data from file is : This is a readme file.
   ```

6. **Try to read from write-only file (should fail)**
   ```bash
   Marvellous CVFS > read 2 20
   Error : Unable to redad as there is no read permission
   ```

7. **List all files**
   ```bash
   Marvellous CVFS > ls
   readme.txt
   config.txt
   temp.txt
   ```

### Tutorial 3: File System Limits

**Objective:** Understand and test the system's capacity limits.

**Steps:**

1. **Create files up to the limit (5 files)**
   ```bash
   Marvellous CVFS > creat file1.txt 3
   File is succesfully created with FD : 0
   
   Marvellous CVFS > creat file2.txt 3
   File is succesfully created with FD : 1
   
   Marvellous CVFS > creat file3.txt 3
   File is succesfully created with FD : 2
   
   Marvellous CVFS > creat file4.txt 3
   File is succesfully created with FD : 3
   
   Marvellous CVFS > creat file5.txt 3
   File is succesfully created with FD : 4
   ```

2. **Try to create a 6th file (should fail)**
   ```bash
   Marvellous CVFS > creat file6.txt 3
   Error : Unable to create file as there is no Inodes
   ```

3. **Test file size limit**
   ```bash
   Marvellous CVFS > write 0
   Please enter the data that you want to write into the file : 
   This is a very long string that exceeds the maximum file size limit of 100 bytes and should cause an error when we try to write it to the file system.
   Error : Insufficient space in tha data block for the file
   ```

4. **Write within limits**
   ```bash
   Marvellous CVFS > write 0
   Please enter the data that you want to write into the file : 
   Short text.
   11 bytes gets succesfully written into the file
   ```

## Common Use Cases

### Use Case 1: Document Management

**Scenario:** Managing small text documents.

**Workflow:**
1. Create documents with read-write permissions
2. Write content to documents
3. Read and verify content
4. List all documents
5. Delete unwanted documents

**Example:**
```bash
# Create documents
creat notes.txt 3
creat todo.txt 3
creat ideas.txt 3

# Write content
write 0  # Enter: "Meeting notes from today"
write 1  # Enter: "1. Finish project 2. Call client"
write 2  # Enter: "New feature ideas for next version"

# Review documents
ls
read 0 20
read 1 15
read 2 25

# Clean up
unlink notes.txt
```

### Use Case 2: Configuration Management

**Scenario:** Managing configuration files with different access levels.

**Workflow:**
1. Create read-only config files
2. Create writable log files
3. Create temporary files for processing
4. Manage access based on file type

**Example:**
```bash
# Create config file (read-only)
creat app.conf 1

# Create log file (write-only)
creat app.log 2

# Create data file (read-write)
creat data.txt 3

# Write to appropriate files
write 1  # Log entries
write 2  # Data content

# Read from appropriate files
read 0 50  # Config file
read 2 50  # Data file
```

### Use Case 3: Testing and Development

**Scenario:** Using CVFS for testing file operations.

**Workflow:**
1. Create test files
2. Perform various operations
3. Test error conditions
4. Verify system behavior
5. Clean up test files

**Example:**
```bash
# Test file creation
creat test1.txt 3
creat test2.txt 1
creat test3.txt 2

# Test operations
write 0  # Valid write
write 1  # Should fail - read-only
write 2  # Valid write

read 0 10  # Valid read
read 1 10  # Should fail - read-only
read 2 10  # Should fail - write-only

# Test error conditions
creat test1.txt 3  # Should fail - file exists
creat test6.txt 3  # Should fail - no inodes (if 5 files exist)

# Clean up
unlink test1.txt
unlink test2.txt
unlink test3.txt
```

## Troubleshooting

### Common Issues and Solutions

#### Issue 1: "Command not found"
**Problem:** Typing an invalid command.

**Solution:**
- Check command spelling
- Use `help` to see available commands
- Use `man <command>` for specific help

**Example:**
```bash
Marvellous CVFS > creatt file.txt 3
Coomand not found...
Please refer Help option or use man command

Marvellous CVFS > help
# Shows correct commands
```

#### Issue 2: "Invalid parameters"
**Problem:** Incorrect command syntax or parameters.

**Solution:**
- Check command syntax
- Verify parameter values
- Use `man <command>` for correct usage

**Example:**
```bash
Marvellous CVFS > creat file.txt 5
Error : Invalid parameters for the function
Please check Man page for more details

Marvellous CVFS > man creat
# Shows valid permissions: 1, 2, 3
```

#### Issue 3: "File already exists"
**Problem:** Trying to create a file with an existing name.

**Solution:**
- Use a different filename
- Delete the existing file first
- Use `ls` to see existing files

**Example:**
```bash
Marvellous CVFS > creat demo.txt 3
File is succesfully created with FD : 0

Marvellous CVFS > creat demo.txt 3
Error : Unable to create file as file is already existing

Marvellous CVFS > ls
demo.txt

Marvellous CVFS > unlink demo.txt
Unlink Opertaion is succesfully performed

Marvellous CVFS > creat demo.txt 3
File is succesfully created with FD : 0
```

#### Issue 4: "No available inodes"
**Problem:** System has reached maximum file capacity.

**Solution:**
- Delete unused files
- Check current files with `ls`
- Plan file usage within limits

**Example:**
```bash
Marvellous CVFS > ls
file1.txt
file2.txt
file3.txt
file4.txt
file5.txt

Marvellous CVFS > creat newfile.txt 3
Error : Unable to create file as there is no Inodes

Marvellous CVFS > unlink file1.txt
Unlink Opertaion is succesfully performed

Marvellous CVFS > creat newfile.txt 3
File is succesfully created with FD : 0
```

#### Issue 5: "Insufficient space"
**Problem:** Trying to write more data than file capacity.

**Solution:**
- Write smaller amounts of data
- Check current file size with `stat`
- Plan data within 100-byte limit

**Example:**
```bash
Marvellous CVFS > stat myfile.txt
Actual File size : 95

Marvellous CVFS > write 0
Please enter the data that you want to write into the file : 
This is a very long string that will exceed the remaining space.
Error : Insufficient space in tha data block for the file

Marvellous CVFS > write 0
Please enter the data that you want to write into the file : 
Short.
5 bytes gets succesfully written into the file
```

### Debugging Tips

1. **Use `ls` frequently** to track current files
2. **Use `stat` to check file details** before operations
3. **Check file descriptors** returned by `creat`
4. **Test with small data first** before large writes
5. **Use `help` and `man`** for command reference

## Best Practices

### File Naming
- Use descriptive names
- Keep names under 49 characters
- Avoid special characters
- Use consistent naming conventions

**Good Examples:**
```bash
creat user_data.txt 3
creat config_backup.txt 1
creat temp_processing.txt 2
```

**Bad Examples:**
```bash
creat a.txt 3  # Too generic
creat very_long_filename_that_exceeds_limit.txt 3  # Too long
creat file@#$%.txt 3  # Special characters
```

### Permission Management
- Use appropriate permissions for file purpose
- Read-only for configuration files
- Write-only for log files
- Read-write for data files

**Permission Guidelines:**
- `1` (Read-only): Configuration files, documentation
- `2` (Write-only): Log files, temporary data
- `3` (Read-write): Data files, user documents

### Data Management
- Plan data size within 100-byte limit
- Use multiple small files instead of one large file
- Regular cleanup of temporary files
- Backup important data before operations

### System Management
- Monitor file count (max 5 files)
- Regular cleanup of unused files
- Plan file usage before starting work
- Use `ls` and `stat` for system monitoring

## Advanced Usage

### Batch Operations
While CVFS doesn't support batch files, you can simulate batch operations:

```bash
# Create multiple files quickly
creat file1.txt 3
creat file2.txt 3
creat file3.txt 3

# Write to all files
write 0  # Enter data for file1
write 1  # Enter data for file2
write 2  # Enter data for file3

# Read from all files
read 0 20
read 1 20
read 2 20
```

### File System Monitoring
Monitor system state and usage:

```bash
# Check current files
ls

# Check file details
stat file1.txt
stat file2.txt

# Check system capacity
# (Create files until you get "no inodes" error)
```

### Error Recovery
Recover from common error conditions:

```bash
# If you get "file already exists"
ls  # See existing files
unlink existing_file.txt  # Remove if needed
creat new_file.txt 3  # Create new file

# If you get "no inodes"
ls  # See all files
unlink unused_file.txt  # Remove unused file
creat new_file.txt 3  # Create new file

# If you get "insufficient space"
stat filename.txt  # Check current size
# Write smaller data or use multiple files
```

---

This user guide provides comprehensive information for using the Marvellous CVFS system effectively. For technical details, refer to the API Documentation and Architecture Documentation.
