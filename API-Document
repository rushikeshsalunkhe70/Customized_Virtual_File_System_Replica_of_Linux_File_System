# Marvellous CVFS - API Documentation

This document provides detailed API documentation for all functions, structures, and constants in the Marvellous CVFS (Custom Virtual File System).

## Table of Contents

- [Constants and Macros](#constants-and-macros)
- [Data Structures](#data-structures)
- [Global Variables](#global-variables)
- [Function Reference](#function-reference)
- [Error Codes](#error-codes)
- [Usage Examples](#usage-examples)

## Constants and Macros

### File System Limits

```cpp
#define MAXFILESIZE 100        // Maximum file size in bytes
#define MAXOPENEDFILES 20      // Maximum number of open files
#define MAXINODE 5             // Maximum number of inodes (files)
```

### File Permissions

```cpp
#define READ 1                 // Read permission
#define WRITE 2                // Write permission
#define EXECUTE 4              // Execute permission
```

### File Types

```cpp
#define REGULARFILE 1          // Regular file type
#define SPECIALFILE 2          // Special file type
```

### Seek Operations

```cpp
#define START 0                // Seek from beginning
#define CURRENT 1              // Seek from current position
#define END 2                  // Seek from end
```

### Success Code

```cpp
#define EXECUTE_SUCCESS 0      // Operation successful
```

## Data Structures

### BootBlock

Contains system boot information.

```cpp
struct BootBlock {
    char Information[100];     // Boot process information
};
```

**Members:**
- `Information[100]`: String containing boot process details

### SuperBlock

Manages file system metadata and inode allocation.

```cpp
struct SuperBlock {
    int TotalInodes;          // Total number of inodes in system
    int FreeInodes;           // Number of available inodes
};
```

**Members:**
- `TotalInodes`: Total number of inodes (set to MAXINODE)
- `FreeInodes`: Number of currently available inodes

### Inode

Represents file metadata and data storage. This is the core structure for file representation.

```cpp
typedef struct Inode {
    char FileName[50];        // Name of the file
    int InodeNumber;          // Unique inode number (1-based)
    int FileSize;             // Allocated file size (MAXFILESIZE)
    int ActualFileSize;       // Actual data size in bytes
    int FileType;             // Type: REGULARFILE or SPECIALFILE
    int ReferenceCount;       // Number of references to this inode
    int LinkCount;            // Number of hard links
    int Permission;           // File permissions (READ, WRITE, or both)
    char *Buffer;             // Data buffer (dynamically allocated)
    struct Inode *next;       // Pointer to next inode in list
} INODE, *PINODE, **PPINODE;
```

**Members:**
- `FileName[50]`: File name (max 49 characters + null terminator)
- `InodeNumber`: Unique identifier (1 to MAXINODE)
- `FileSize`: Always set to MAXFILESIZE (100 bytes)
- `ActualFileSize`: Current amount of data stored
- `FileType`: Type of file (currently only REGULARFILE supported)
- `ReferenceCount`: Number of file descriptors pointing to this inode
- `LinkCount`: Number of directory entries pointing to this inode
- `Permission`: Access permissions (1=Read, 2=Write, 3=Read+Write)
- `Buffer`: Dynamically allocated memory for file data
- `next`: Pointer to next inode in the linked list

### FileTable

Tracks information about open files.

```cpp
typedef struct FileTable {
    int ReadOffset;           // Current read position
    int WriteOffset;          // Current write position
    int Count;                // Reference count
    int Mode;                 // Access mode (permissions)
    PINODE ptrinode;          // Pointer to associated inode
} FILETABLE, *PFILETABLE;
```

**Members:**
- `ReadOffset`: Current position for read operations
- `WriteOffset`: Current position for write operations
- `Count`: Number of references to this file table entry
- `Mode`: Access mode (same as inode permission)
- `ptrinode`: Pointer to the associated inode

### UAREA

Process-specific user area containing file descriptor table.

```cpp
struct UAREA {
    char ProcessName[50];                    // Process name
    PFILETABLE UFDT[MAXOPENEDFILES];        // User File Descriptor Table
};
```

**Members:**
- `ProcessName[50]`: Name of the process (set to "Myexe")
- `UFDT[MAXOPENEDFILES]`: Array of file table pointers (file descriptors)

## Global Variables

```cpp
BootBlock bootobj;           // Global boot block instance
SuperBlock superobj;         // Global super block instance
PINODE head = NULL;          // Head of inode linked list
UAREA uareaobj;              // Global user area instance
```

## Function Reference

### System Initialization Functions

#### StartAuxilaryDataInitialisation()

Initializes the entire file system.

```cpp
void StartAuxilaryDataInitialisation();
```

**Description:** Master initialization function that sets up all file system components.

**Process:**
1. Initializes boot block with boot message
2. Calls `InitialiseSuperblock()`
3. Calls `CreateDILB()`
4. Calls `InitialiseUAREA()`

**Output:** Console messages confirming each initialization step.

---

#### InitialiseSuperblock()

Initializes the super block with inode counts.

```cpp
void InitialiseSuperblock();
```

**Description:** Sets up the super block with total and free inode counts.

**Process:**
- Sets `TotalInodes` to `MAXINODE` (5)
- Sets `FreeInodes` to `MAXINODE` (5)

**Output:** Confirmation message to console.

---

#### CreateDILB()

Creates the Directory Inode List Block (linked list of inodes).

```cpp
void CreateDILB();
```

**Description:** Allocates and initializes all available inodes in a linked list.

**Process:**
1. Creates `MAXINODE` number of inode structures
2. Initializes each inode with default values
3. Links them in a singly linked list
4. Sets `head` pointer to first inode

**Default Inode Values:**
- `InodeNumber`: Sequential from 1 to MAXINODE
- `FileSize`: 0
- `ActualFileSize`: 0
- `LinkCount`: 0
- `Permission`: 0
- `FileType`: 0
- `ReferenceCount`: 0
- `Buffer`: NULL
- `next`: NULL

**Output:** Confirmation message to console.

---

#### InitialiseUAREA()

Initializes the user area and file descriptor table.

```cpp
void InitialiseUAREA();
```

**Description:** Sets up the process-specific user area.

**Process:**
1. Sets process name to "Myexe"
2. Initializes all UFDT entries to NULL

**Output:** Confirmation message to console.

### File Operation Functions

#### CreateFile()

Creates a new regular file.

```cpp
int CreateFile(char *name, int permission);
```

**Parameters:**
- `name`: Name of the file to create (cannot be NULL)
- `permission`: File permissions (1=Read, 2=Write, 3=Read+Write)

**Returns:**
- File descriptor (0 to MAXINODE-1) on success
- Error code on failure

**Error Codes:**
- `ERR_INVALID_PARAMETER`: Invalid name or permission
- `ERR_NO_INODES`: No available inodes
- `ERR_FILE_ALREADY_EXIST`: File with same name exists

**Process:**
1. Validates input parameters
2. Checks for available inodes
3. Checks if file already exists
4. Finds first free inode
5. Allocates file table entry
6. Initializes inode and file table
7. Allocates buffer memory
8. Decrements free inode count

**Example:**
```cpp
int fd = CreateFile("demo.txt", 3);
if (fd >= 0) {
    printf("File created with FD: %d\n", fd);
}
```

---

#### UnlinkFile()

Deletes a file from the system.

```cpp
int UnlinkFile(char *name);
```

**Parameters:**
- `name`: Name of the file to delete (cannot be NULL)

**Returns:**
- `EXECUTE_SUCCESS` (0) on success
- Error code on failure

**Error Codes:**
- `ERR_INVALID_PARAMETER`: Invalid name
- `ERR_FILE_NOT_EXIST`: File does not exist

**Process:**
1. Validates input parameter
2. Checks if file exists
3. Finds file in UFDT
4. Frees buffer memory
5. Resets inode values
6. Frees file table memory
7. Sets UFDT entry to NULL
8. Increments free inode count

**Example:**
```cpp
int result = UnlinkFile("demo.txt");
if (result == EXECUTE_SUCCESS) {
    printf("File deleted successfully\n");
}
```

---

#### write_file()

Writes data to a file.

```cpp
int write_file(int fd, char *data, int size);
```

**Parameters:**
- `fd`: File descriptor (0 to MAXOPENEDFILES-1)
- `data`: Data to write (cannot be NULL)
- `size`: Number of bytes to write

**Returns:**
- Number of bytes written on success
- Error code on failure

**Error Codes:**
- `ERR_INVALID_PARAMETER`: Invalid file descriptor
- `ERR_FILE_NOT_EXIST`: File not open
- `ERR_PERMISSION_DENIED`: No write permission
- `ERR_INSUFFICIENT_SPACE`: Not enough space

**Process:**
1. Validates file descriptor
2. Checks if file is open
3. Verifies write permission
4. Checks available space
5. Copies data to buffer at write offset
6. Updates write offset
7. Updates actual file size

**Example:**
```cpp
char data[] = "Hello, World!";
int bytes = write_file(0, data, strlen(data));
printf("Wrote %d bytes\n", bytes);
```

---

#### read_file()

Reads data from a file.

```cpp
int read_file(int fd, char *data, int size);
```

**Parameters:**
- `fd`: File descriptor (0 to MAXOPENEDFILES-1)
- `data`: Buffer to store read data (cannot be NULL)
- `size`: Number of bytes to read

**Returns:**
- Number of bytes read on success
- Error code on failure

**Error Codes:**
- `ERR_INVALID_PARAMETER`: Invalid parameters
- `ERR_FILE_NOT_EXIST`: File not open
- `ERR_PERMISSION_DENIED`: No read permission
- `ERR_INSUFFICIENT_DATA`: Not enough data

**Process:**
1. Validates parameters
2. Checks if file is open
3. Verifies read permission
4. Checks available data
5. Copies data from buffer at read offset
6. Updates read offset

**Example:**
```cpp
char buffer[100];
int bytes = read_file(0, buffer, 10);
if (bytes > 0) {
    buffer[bytes] = '\0';
    printf("Read: %s\n", buffer);
}
```

---

#### ls_file()

Lists all files in the system.

```cpp
void ls_file();
```

**Description:** Displays names of all existing files.

**Process:**
1. Traverses inode linked list
2. Prints filename for each file with FileType != 0

**Output:** File names printed to console, one per line.

**Example:**
```cpp
ls_file();
// Output:
// demo.txt
// test.txt
```

---

#### stat_file()

Displays detailed file statistics.

```cpp
int stat_file(char *name);
```

**Parameters:**
- `name`: Name of the file (cannot be NULL)

**Returns:**
- `EXECUTE_SUCCESS` (0) on success
- Error code on failure

**Error Codes:**
- `ERR_INVALID_PARAMETER`: Invalid name
- `ERR_FILE_NOT_EXIST`: File does not exist

**Process:**
1. Validates input parameter
2. Checks if file exists
3. Finds file in inode list
4. Displays detailed statistics

**Output:** Formatted file information including:
- File name
- File size on disk
- Actual file size
- Link count
- File permissions
- File type

**Example:**
```cpp
int result = stat_file("demo.txt");
if (result == EXECUTE_SUCCESS) {
    // Statistics displayed to console
}
```

### Utility Functions

#### IsFileExists()

Checks if a file exists in the system.

```cpp
bool IsFileExists(char *name);
```

**Parameters:**
- `name`: Name of the file to check

**Returns:**
- `true` if file exists and is a regular file
- `false` if file does not exist

**Process:**
1. Traverses inode linked list
2. Compares filename and checks FileType == REGULARFILE
3. Returns result

**Example:**
```cpp
if (IsFileExists("demo.txt")) {
    printf("File exists\n");
}
```

---

#### DisplayHelp()

Displays help information for all commands.

```cpp
void DisplayHelp();
```

**Description:** Shows usage information for all available commands.

**Output:** Formatted help text with command descriptions and usage.

---

#### ManPage()

Displays manual page for a specific command.

```cpp
void ManPage(char *name);
```

**Parameters:**
- `name`: Name of the command

**Description:** Shows detailed manual information for the specified command.

**Supported Commands:**
- `creat`: File creation command
- `exit`: Exit command
- `unlink`: File deletion command
- `stat`: File statistics command
- `ls`: List files command
- `write`: Write data command
- `read`: Read data command

**Output:** Detailed manual page with description, usage, and parameters.

## Error Codes

| Code | Name | Value | Description |
|------|------|-------|-------------|
| `ERR_INVALID_PARAMETER` | -1 | Invalid function parameters |
| `ERR_NO_INODES` | -2 | No available inodes |
| `ERR_FILE_ALREADY_EXIST` | -3 | File already exists |
| `ERR_FILE_NOT_EXIST` | -4 | File does not exist |
| `ERR_PERMISSION_DENIED` | -5 | Insufficient permissions |
| `ERR_INSUFFICIENT_SPACE` | -6 | Not enough space |
| `ERR_INSUFFICIENT_DATA` | -7 | Not enough data to read |

## Usage Examples

### Complete File Operations Example

```cpp
#include "program724.cpp"

int main() {
    // Initialize the file system
    StartAuxilaryDataInitialisation();
    
    // Create a file
    int fd = CreateFile("example.txt", 3);
    if (fd < 0) {
        printf("Failed to create file\n");
        return -1;
    }
    
    // Write data to file
    char data[] = "Hello, World!";
    int bytes_written = write_file(fd, data, strlen(data));
    printf("Wrote %d bytes\n", bytes_written);
    
    // Read data from file
    char buffer[100];
    int bytes_read = read_file(fd, buffer, 10);
    if (bytes_read > 0) {
        buffer[bytes_read] = '\0';
        printf("Read: %s\n", buffer);
    }
    
    // List all files
    ls_file();
    
    // Show file statistics
    stat_file("example.txt");
    
    // Delete the file
    UnlinkFile("example.txt");
    
    return 0;
}
```

### Error Handling Example

```cpp
int fd = CreateFile("test.txt", 3);

if (fd == ERR_INVALID_PARAMETER) {
    printf("Error: Invalid parameters\n");
} else if (fd == ERR_NO_INODES) {
    printf("Error: No available inodes\n");
} else if (fd == ERR_FILE_ALREADY_EXIST) {
    printf("Error: File already exists\n");
} else if (fd >= 0) {
    printf("File created successfully with FD: %d\n", fd);
}
```

---

This API documentation provides comprehensive information about all functions, structures, and constants in the Marvellous CVFS system. For additional examples and usage patterns, refer to the main README.md file.
