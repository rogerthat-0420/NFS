# NETWORK FILE SYSTEM
The repository is an implementation of a Network File System.
The project was a part of the **CS3.301 -  Operating Systems and Networks** course
The link to the complete problem statement is [here](https://karthikv1392.github.io/cs3301_osn/project/).

# Components
### 1. Clients

- Writing to a File
- Reading a File
- Deleting a File/Folder
- Creating a File/Folder
- Listing All Files and Folders in a Folder
- Getting Additional Information (Size and Permissions)

### 2. Naming Server (NM)

- Initialization
- Storage Server Data Handling

### 3. Storage Servers (SS)

- Dynamic Additions
- Commands Issued by NM
- Client Interactions

# Features 
### 1. Naming and Storage Servers Initialization

- Initialize Naming Server and Storage Servers
- Storage Servers send vital details to the Naming Server
- Naming Server starts accepting client requests

### 2. Commands Issued by NM on Storage Servers

- Create empty files/directories
- Delete files/directories
- Copy files/directories from one Storage Server to another

### 3. Client Interactions

- Facilitate client requests for reading, writing, and retrieving information about files
- Support client requests for creating and deleting files/folders
- Enable copying files/directories between Storage Servers

### 4. Multiple Clients

- Allow concurrent client access to the Naming Server
- Enable concurrent file reading by multiple clients, while write operations are exclusive (readers-writer lock used)


### 5. Search in Naming Servers

- Optimize search processes using efficient data structures (Tries)
- Implement LRU caching for recent searches to enhance response times

### 6. Redundancy/Replication

- Detect Storage Server failures for prompt response
- Implement data redundancy and replication strategy for fault tolerance
- Allow asynchronous duplication of read commands across replicated stores

### 7. Bookkeeping

- Implement logging mechanism for recording requests, acknowledgments, and outcomes
- Include relevant information in logs, such as IP addresses and ports used in communications

# Assumptions
- No invalid file paths are enterred by the user.
- Copying files works only for *.txt* files.
- A new file which has not been created using the **CREATE** command can not be written to using the **WRITE** command.
- User can not write to the back up server (which has a copy of the file) when the main server (with the original file) is down. Read operations are 
- For copying files, the source path and the destination path both need to be paths to the source and destination files. The destination file path must be that of a file which has been already created.
  
# Testing
You can play around with the `TESTING` folder.
- Initialize `SS`<sub>`i`</sub> in `diri`
- Try out the 6 [client operations](#1-clients).
- Try reading large files (upto 25 MB in size).
- Copy from a file/directory in `SS`<sub>`i`</sub> to a file/directory in `SS`<sub>`j`</sub>
- Initiate more than 3 `SS` to test out the implementation of Redundancy / Back up in servers.
- Make 2 clients simultaneously read/write:
    - read-read
    - read-write
    - write-write
- Try connecting/disconnecting clients and storage servers from the naming server.
- Experiment with the different implmentations for `accessible paths`. Jump to [Notes](#notes) for more.

# Team Members
- [Aditya Mishra](https://github.com/AdityaMishraOG)
- [Hardik Kalia](https://github.com/hardikkalia)
- [Tejas Cavale](https://github.com/Tele29)
  
# Notes
The implementation of a storage server is very flexible for `accessible paths` -

1. ```c
    // IMPLEMENTATION NO 1 -> SEND N ACCESSIBLE PATHS
    int num_accessible_paths;
    printf("Enter number of accessible paths: ");
    scanf("%d", &num_accessible_paths);
    for (int i = 0; i < num_accessible_paths; i++)
    {
        char accessible_path[BUFFER_SIZE];
        scanf("%s", accessible_path);

        send(socket_fd, no_prefix(accessible_path), BUFFER_SIZE, 0);
    }
    ```

2. ```c
    // IMPLEMENTATION NO 2 -> SEND ALL PATHS IN CWD AS ACCESSIBLE PATHS RECURSIVELY
    char starting_directory[4096] = ".";
    seek_recursive_send_dir_trie(socket_fd, starting_directory, ".");
    ```

Comment out one implementation and uncomment the other to experiment with the possibilities. (Jump to *line 538* in in `storage_server.c`)
