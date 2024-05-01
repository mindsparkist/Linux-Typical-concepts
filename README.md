The `find` command is a powerful tool in Linux for searching for files and directories based on various criteria. Here's a breakdown of its usage and common use cases:

**Basic Syntax:**

```bash
find [start_directory] [options] [expression]
```

- `find`: The command itself.
- `[start_directory]`: (Optional) The directory to start searching from. Defaults to the current directory if omitted.
- `[options]`: (Optional) Flags to modify the search behavior (more on this later).
- `[expression]`: Criteria for filtering and locating files/directories.

**Common Options:**

* `-name <filename>`: Find files with a specific name (case-sensitive by default).
* `-iname <filename>`: Find files with a specific name, ignoring case.
* `-type f`: Find files (as opposed to directories).
* `-type d`: Find directories.
* `-size <size>[ckM]` (or `-size +<size>`, `-size -<size>`): Find files based on size (e.g., `-size 10M` for files larger than 10 MB).
* `-perm <permission>`: Find files with specific permissions (requires understanding permission codes).
* `-user <username>`: Find files owned by a specific user.
* `-group <groupname>`: Find files owned by a specific group.
* `-mtime <days>` (or `-atime <days>`, `-ctime <days>`): Find files modified (mtime), accessed (atime), or changed inode (ctime) within the last <days> days.
* `-exec <command> {} \;`: Execute a command on each file found (replace `<command>` with the desired action and `{}` represents the filename).

**Example Use Cases:**

1. **Find a Specific File:**

```bash
find . -name "myfile.txt"
```

This searches for a file named "myfile.txt" in the current directory (`.`) and any subdirectories.

2. **Find Files Ignoring Case:**

```bash
find /home -iname "*.jpg"
```

This searches for all JPEG image files (`.jpg` extension) in the `/home` directory and its subdirectories, ignoring case (uppercase or lowercase).

3. **Find All Files Larger Than 10 MB:**

```bash
find /data -type f -size +10M
```

This searches for all files larger than 10 MB within the `/data` directory and its subdirectories.

4. **Find Files Modified in the Last Week:**

```bash
find /var/log -mtime -7
```

This searches for all files in the `/var/log` directory (often used for system logs) that were modified within the last 7 days.

5. **Delete Empty Files (Use with Caution!):**

```bash
find . -type f -empty -delete
```

This searches for empty files (zero bytes) in the current directory and its subdirectories and deletes them. **Important:** Be cautious when using `-delete` as it's permanent. It's recommended to test with `-print` first to see which files would be deleted.

**Additional Tips:**

* Combine multiple options for more specific searches. For example, `find . -type f -name "*.txt" -mtime -1` finds text files modified yesterday in the current directory.
* Use `man find` for the complete manual page with detailed information on all options and examples.
* The `locate` command can be a faster alternative for finding files by filename, but it relies on a pre-built database that needs to be updated regularly.

By mastering the `find` command, you'll gain a valuable skill for efficiently locating and managing files in your Linux system.

There are a couple of ways to check if you have sudo access on Ubuntu:

**1. Try running a command with sudo:**

The most straightforward way is to try running a command that requires root privileges with sudo. If you have sudo access, it will prompt you for your password and then execute the command. If you don't have sudo access, you'll see an error message like "Sorry, user [your_username] may not run sudo on this machine."

**2. Check group membership:**

On Ubuntu, users with sudo access typically belong to the "sudo" group. You can check your group membership using the `groups` command. If the output includes "sudo", then you likely have sudo access.

Here's the command:

```
groups
```

**Important note:**  While checking group membership can be a clue, it's not definitive proof of sudo access.  A user could be in the "sudo" group but not have actual sudo privileges due to the configuration in the sudoers file.

**Caution:** Editing the `/etc/sudoers` file directly is not recommended as it can lead to security vulnerabilities if done incorrectly. If you need to grant sudo access to a user, it's best to consult the Ubuntu documentation or seek help from a system administrator.

The methods for checking sudo access in RHEL are similar to Ubuntu, but with a slight difference:

**1. Try running a command with sudo:**

As in Ubuntu, attempt to run a command that requires root privileges with sudo. If you have sudo access, it will prompt for your password and execute the command. Otherwise, you'll see an error message like `"Sorry, user [your_username] may not run sudo on this machine."`

**2. Check group membership:**

In RHEL, users with sudo access also belong to the "sudo" group. You can verify your group membership using the `groups` command. If the output includes "sudo", then you likely have sudo access.

Here's the command:

```
groups
```

**Important note:** Similar to Ubuntu, being in the "sudo" group is an indicator but not a guarantee of sudo access. The sudoers file ultimately determines user privileges.

**Additional method (RHEL-specific):**

*RHEL provides a way to check sudo privileges without actually running a command.* You can use the `sudo -l` flag to list your allowed sudo privileges. However, this method might have security restrictions:

```
sudo -l
```

If you have sudo access, it will display the commands you're authorized to run with sudo. If you don't have access or the security policy disallows it, you'll see an error message like `"User [your_username] is not allowed to run sudo on [hostname]"`

**Caution:** Editing the `/etc/sudoers` file directly to grant sudo access is not recommended due to potential security risks. Consult the RHEL documentation or seek help from a system administrator for proper user permission management.

The `fsck` (File System Consistency Check) is a crucial tool in RHEL (Red Hat Enterprise Linux) for checking and repairing potential errors in file system structures. Here's a breakdown of its functionality and how to use it:

**Purpose:**

* The `fsck` command scans a specified file system for inconsistencies or damage that might arise due to unexpected shutdowns, power outages, or hardware failures.
* If errors are detected, `fsck` can attempt to automatically fix them, restoring the integrity of the file system.

**Common Use Cases:**

* **Boot Time Checks:** RHEL automatically runs `fsck` on critical file systems (like the root partition) during the boot process to ensure a healthy file system state before continuing.
* **Manual Checks:** You can also use `fsck` manually to diagnose and potentially repair issues in file systems, especially after unexpected system crashes.

**Important Note:**

Running `fsck` with certain options, particularly `-a` (automatic repair), can potentially cause data loss if the file system is severely damaged. It's recommended to proceed with caution and ideally have backups available before running `fsck` for repairs.

**Using `fsck` in RHEL:**

The basic syntax for `fsck` is:

```bash
fsck <options> <file_system>
```

* `<options>`: Specify options that control the behavior of `fsck`. Some common options include:
    * `-a`: Automatically repair detected errors (use with caution).
    * `-p`:  Prompt the user for confirmation before attempting repairs.
    * `-t <type>`: Specify the file system type (e.g., `-t ext4`).
* `<file_system>`:  This can be represented by:
    * The device path of the partition (e.g., `/dev/sda1`).
    * The mount point of the file system (e.g., `/home`).

**Here are some examples of using `fsck` in RHEL:**

* **Check and Repair the Root File System (Automatic):**

   **Warning:** This should only be done in rescue mode, **not** during normal boot.

   ```bash
   fsck -a /dev/sda1  # Replace /dev/sda1 with your actual root partition device
   ```

* **Check the Root File System (Prompting for Repairs):**

   ```bash
   fsck -p /dev/sda1
   ```

   This checks the root file system and prompts you to confirm any repairs before proceeding.

* **Check a Specific File System by Mount Point:**

   ```bash
   fsck /home  # Assuming /home is mounted on a separate partition
   ```

**Additional Notes:**

* The specific file system type (e.g., ext4, xfs) might influence the available options and behavior of `fsck`. It's recommended to consult the documentation for your specific file system for detailed information.
* In RHEL, the file system check during boot is often handled by fsck.service or similar systemd services. You can usually find logs related to fsck checks in `/var/log/messages`.

By understanding the `fsck` command and its capabilities, you can effectively diagnose and potentially repair file system issues in your RHEL system. Remember to exercise caution, especially when using automatic repair options, and have backups in place whenever possible.


Both `df -h` and `blkid` are useful commands in Linux for managing storage devices and file systems, but they serve different purposes:

**df (Disk Free):**

* **Function:** `df` (with the `-h` option for human-readable output) displays information about mounted file systems.
* **Output:** It provides details like:
    * **Filesystem:** The name of the file system (e.g., /dev/sda1)
    * **Size:** Total size of the file system
    * **Used:** Amount of space currently used on the file system
    * **Available:** Amount of free space remaining on the file system
    * **Use%:** Percentage of used space relative to the total size
    * **Mounted on:** The directory where the file system is mounted (mount point)

**Use Case:**

Use `df -h` to get a quick overview of available disk space on mounted file systems. This helps you determine if you're running low on storage and need to free up space or add additional storage.

**blkid (Block Device Identifier):**

* **Function:** `blkid` displays information about block devices, including their unique identifiers like UUID (Universally Unique Identifier) and labels.
* **Output:** It shows details like:
    * **Device name:** Path to the block device (e.g., /dev/sda1)
    * **UUID:** A unique identifier for the device
    * **TYPE:** File system type formatted on the device (e.g., ext4, xfs)
    * **LABEL:** A human-readable name assigned to the file system (if any)

**Use Case:**

Use `blkid` to identify the specific block devices and their unique identifiers. This information is useful for:
    * Mounting file systems using UUID or label instead of device paths in `/etc/fstab` (more reliable across reboots).
    * Identifying partitions or disks, especially when dealing with multiple storage devices.
    * Recovering data in situations where device names might change.

**Here's a table summarizing the key differences:**

| Feature        | df -h                                         | blkid                                        |
|----------------|----------------------------------------------|-----------------------------------------------|
| **Function**     | Shows information about mounted file systems   | Shows information about block devices          |
| **Output**       | Size, Used, Available, Mount point             | UUID, TYPE, LABEL (if any)                     |
| **Useful for**   | Checking available disk space                 | Identifying devices, using UUIDs/Labels for mounting |

**In Conclusion:**

* Use `df -h` to monitor disk space usage on mounted file systems.
* Use `blkid` to identify block devices, their types, and unique identifiers for better management and mounting flexibility.

Both commands are valuable tools for understanding and managing storage in your Linux system. They provide complementary information for different purposes.

The `free -m` command in Linux displays information about memory usage on your system, with the output shown in megabytes (M). Here's a breakdown of what it shows and how it can be helpful:

**Output:**

The `free -m` command typically displays three main sections:

1. **Total:** This line shows the total amount of memory available on your system in Megabytes (MiB).
2. **Used:** This line shows the amount of memory currently in use by applications, the kernel, and other processes, also in Megabytes.
3. **Free:** This line shows the amount of memory that is currently free and available for allocation, in Megabytes.

**Additional Information (Optional):**

Depending on your system configuration, `free -m` might also display:

* **Shared:** Memory that can be shared between multiple processes.
* **Buffers:** Memory used for buffering by the kernel to improve performance.
* **Cached:** Memory used for caching data to speed up access.
* **Available:** This might be a calculated value based on Free + Buffers + Cached, representing the total usable memory.

**Understanding Memory Usage:**

By analyzing the output of `free -m`, you can gain insights into your system's memory utilization. Here's what each value indicates:

* **High Used:** If the Used value is close to the Total value, it might indicate that your system is running low on memory and applications might experience performance issues. You could consider closing unnecessary programs or adding more RAM.
* **Sufficient Free:** A healthy amount of Free memory ensures your system has room to run applications smoothly. However, a very high Free value might not necessarily be bad, as the system might be efficiently caching data for faster access.

**Using `free -m` Effectively:**

* The `free -m` command is a quick way to get a snapshot of your system's memory usage at a specific point in time.
* To monitor memory usage over time, consider using tools like `htop` or `watch free -m` (which repeatedly runs `free -m` with a short delay).
* For a more detailed breakdown of memory usage, including information about memory allocation by processes, you can explore tools like `vmstat` or `top`.

**In Conclusion:**

The `free -m` command is a handy tool for understanding basic memory usage on your Linux system. By monitoring memory utilization, you can make informed decisions about resource allocation and application management to ensure smooth system performance.

The `fsck` (File System Consistency Check) is a crucial tool in RHEL (Red Hat Enterprise Linux) for checking and repairing potential errors in file system structures. Here's a breakdown of its functionality and how to use it:

**Purpose:**

* The `fsck` command scans a specified file system for inconsistencies or damage that might arise due to unexpected shutdowns, power outages, or hardware failures.
* If errors are detected, `fsck` can attempt to automatically fix them, restoring the integrity of the file system.

**Common Use Cases:**

* **Boot Time Checks:** RHEL automatically runs `fsck` on critical file systems (like the root partition) during the boot process to ensure a healthy file system state before continuing.
* **Manual Checks:** You can also use `fsck` manually to diagnose and potentially repair issues in file systems, especially after unexpected system crashes.

**Important Note:**

Running `fsck` with certain options, particularly `-a` (automatic repair), can potentially cause data loss if the file system is severely damaged. It's recommended to proceed with caution and ideally have backups available before running `fsck` for repairs.

**Using `fsck` in RHEL:**

The basic syntax for `fsck` is:

```bash
fsck <options> <file_system>
```

* `<options>`: Specify options that control the behavior of `fsck`. Some common options include:
    * `-a`: Automatically repair detected errors (use with caution).
    * `-p`:  Prompt the user for confirmation before attempting repairs.
    * `-t <type>`: Specify the file system type (e.g., `-t ext4`).
* `<file_system>`:  This can be represented by:
    * The device path of the partition (e.g., `/dev/sda1`).
    * The mount point of the file system (e.g., `/home`).

**Here are some examples of using `fsck` in RHEL:**

* **Check and Repair the Root File System (Automatic):**

   **Warning:** This should only be done in rescue mode, **not** during normal boot.

   ```bash
   fsck -a /dev/sda1  # Replace /dev/sda1 with your actual root partition device
   ```

* **Check the Root File System (Prompting for Repairs):**

   ```bash
   fsck -p /dev/sda1
   ```

   This checks the root file system and prompts you to confirm any repairs before proceeding.

* **Check a Specific File System by Mount Point:**

   ```bash
   fsck /home  # Assuming /home is mounted on a separate partition
   ```

**Additional Notes:**

* The specific file system type (e.g., ext4, xfs) might influence the available options and behavior of `fsck`. It's recommended to consult the documentation for your specific file system for detailed information.
* In RHEL, the file system check during boot is often handled by fsck.service or similar systemd services. You can usually find logs related to fsck checks in `/var/log/messages`.

By understanding the `fsck` command and its capabilities, you can effectively diagnose and potentially repair file system issues in your RHEL system. Remember to exercise caution, especially when using automatic repair options, and have backups in place whenever possible.

The `free -m` command in Linux displays information about memory usage on your system, with the output shown in megabytes (M). Here's a breakdown of what it shows and how it can be helpful:

**Output:**

The `free -m` command typically displays three main sections:

1. **Total:** This line shows the total amount of memory available on your system in Megabytes (MiB).
2. **Used:** This line shows the amount of memory currently in use by applications, the kernel, and other processes, also in Megabytes.
3. **Free:** This line shows the amount of memory that is currently free and available for allocation, in Megabytes.

**Additional Information (Optional):**

Depending on your system configuration, `free -m` might also display:

* **Shared:** Memory that can be shared between multiple processes.
* **Buffers:** Memory used for buffering by the kernel to improve performance.
* **Cached:** Memory used for caching data to speed up access.
* **Available:** This might be a calculated value based on Free + Buffers + Cached, representing the total usable memory.

**Understanding Memory Usage:**

By analyzing the output of `free -m`, you can gain insights into your system's memory utilization. Here's what each value indicates:

* **High Used:** If the Used value is close to the Total value, it might indicate that your system is running low on memory and applications might experience performance issues. You could consider closing unnecessary programs or adding more RAM.
* **Sufficient Free:** A healthy amount of Free memory ensures your system has room to run applications smoothly. However, a very high Free value might not necessarily be bad, as the system might be efficiently caching data for faster access.

**Using `free -m` Effectively:**

* The `free -m` command is a quick way to get a snapshot of your system's memory usage at a specific point in time.
* To monitor memory usage over time, consider using tools like `htop` or `watch free -m` (which repeatedly runs `free -m` with a short delay).
* For a more detailed breakdown of memory usage, including information about memory allocation by processes, you can explore tools like `vmstat` or `top`.

**In Conclusion:**

The `free -m` command is a handy tool for understanding basic memory usage on your Linux system. By monitoring memory utilization, you can make informed decisions about resource allocation and application management to ensure smooth system performance.

Both `#!/bin/bash` and `#!/bin/sh` are shebang lines, which are used in scripting languages like Bash to specify the interpreter that should be used to execute the script. Here's a breakdown of the differences and when to use each:

**#!/bin/bash:**

* **Interpreter:** This specifies the Bash shell interpreter located at `/bin/bash`.
* **Functionality:**  This line tells the system to use the full features and functionalities of the Bash shell to execute the script. Bash is a powerful shell with many built-in commands, control structures, and scripting capabilities.
* **Use Case:** Use this shebang line when your script relies on features specific to Bash, such as:
    * Bash arrays
    * Advanced conditional statements ([[ ]] syntax)
    * Bash-specific functions and keywords

**#!/bin/sh:**

* **Interpreter:** This line traditionally specifies the generic Bourne shell interpreter, typically located at `/bin/sh`. However, on many modern Linux distributions, `/bin/sh` is a symbolic link that points to another shell, often Bash itself.
* **Functionality:** This line instructs the system to use a more basic shell interpreter to execute the script. This interpreter might have a limited feature set compared to Bash.
* **Use Case:** Use this shebang line when:
    * Your script needs to be compatible with a wider range of systems, especially older ones that might not have Bash installed by default.
    * You only use basic shell functionalities like variable manipulation, simple control flow (if/else), and common shell commands that are POSIX-compliant (part of the Portable Operating System Interface standard).

**Here's a table summarizing the key differences:**

| Shebang Line          | Interpreter                               | Functionality                                 | Use Case                                          |
|------------------------|----------------------------------------------|-------------------------------------------------|----------------------------------------------------|
| #!/bin/bash            | Bash shell interpreter (`/bin/bash`)          | Full Bash features and functionalities           | Scripts using Bash-specific features              |
| #!/bin/sh              | Bourne shell interpreter (`/bin/sh`) (might   | More basic shell functionalities (may vary)      | Portable scripts, wider system compatibility      |
|                        | be a symbolic link to Bash on some systems)   |                                                 |                                                    |

**Additional Considerations:**

* Even if `/bin/sh` points to Bash on your system, using `#!/bin/bash` explicitly ensures your script leverages the full capabilities of Bash.
* For maximum portability and compatibility, consider using `#!/bin/sh` and avoid using Bash-specific features in your script. If you need those features, document the requirement for a Bash interpreter.

By understanding the distinction between `#!/bin/bash` and `#!/bin/sh`, you can choose the appropriate shebang line for your scripts, ensuring compatibility and functionality across different systems.

An inode (index node) is a fundamental data structure in Linux filesystems. It acts like an index entry for a file, storing essential information about the file,  even though the actual data content is stored elsewhere on the disk. Here's a breakdown of inodes and how they relate to disk space:

**What an Inode Contains:**

* **File system information:** File type (regular file, directory, symbolic link, etc.).
* **File permissions:** Read, write, and execute permissions for owner, group, and others.
* **File ownership:** User and group that own the file.
* **Timestamps:** Last access time, modification time, and inode change time.
* **File size:** The size of the data the file holds.
* **Data block pointers:** Locations (block numbers) on the disk where the actual file content is stored. Inodes typically reference a limited number of data blocks directly, and larger files might use additional indirect blocks to manage extensive data.

**Inodes vs. Disk Space:**

* **Separate Concepts:** Inodes and disk space are distinct concepts. Inodes track file metadata, while disk space refers to the actual storage capacity used by the file's data content.
* **Limited Inodes:** The number of inodes is a fixed value set when a filesystem is created. It's typically calculated based on the filesystem size, but the exact ratio can vary.
* **Running Out of Inodes:** It's possible to run out of inodes before exhausting all disk space. This can happen if you have a large number of small files, each consuming one inode even with minimal data.

**Example Scenario:**

Imagine you have a 1 GB partition with 1 million inodes. You create 1 million files, each containing just 1 byte of data. In this case:

* **Disk Space Usage:** The total data used by these files is only 1 MB (1 million bytes). You still have a significant amount of free disk space (almost 1 GB).
* **Inode Usage:** However, you've used up all 1 million inodes, as each file requires one, regardless of its size. You cannot create any more files, even though there's ample free disk space.

**In Conclusion:**

Understanding inodes is crucial for managing your filesystem effectively. While disk space represents the overall storage capacity, inodes act like a limited pool of entries to manage files. If you work with a large number of small files, monitor inode usage to avoid running out before you exhaust disk space. Consider strategies like file archiving, compression, or using filesystems with a higher inode allocation if needed.

In computer programming,  standard input (STDIN), standard output (STDOUT), and standard error (STDERR) are fundamental concepts related to how a program interacts with its environment,  especially the user's terminal or console. Here's a detailed explanation of each:

**Standard Input (STDIN):**

* **Function:** STDIN represents the source from which a program receives its input data.
* **Default Source:** By default, STDIN is usually connected to the keyboard, meaning the user can type input that the program reads and processes.
* **Redirection:** STDIN can be redirected from the keyboard to come from a file using the `<` operator. For example, `cat <myfile.txt` redirects the contents of `myfile.txt` to STDIN as input for the `cat` command.

**Standard Output (STDOUT):**

* **Function:** STDOUT represents the destination where a program sends its output data.
* **Default Destination:** By default, STDOUT is usually connected to the terminal or console window, making the program's output visible to the user.
* **Redirection:** STDOUT can be redirected to a file using the `>` operator. For example, `ls > directory_listing.txt` redirects the output of the `ls` command (list directory contents) to a file named `directory_listing.txt`.

**Standard Error (STDERR):**

* **Function:** STDERR represents a separate output stream for error messages or diagnostic information produced by a program.
* **Differentiation from STDOUT:**  While STDOUT is intended for the program's regular output, STDERR is specifically for error messages and warnings. This distinction helps users differentiate between the program's results and any errors encountered during execution.
* **Default Destination:**  Similar to STDOUT, STDERR is also typically connected to the terminal by default, so error messages are displayed alongside the regular output.
* **Redirection:**  STDERR can also be redirected to a file using the `2>` operator (or `>&2` for appending to an existing file).  For example, `command_name 2> errors.txt` redirects error messages from `command_name` to a file named `errors.txt`.

**Here's an analogy to further understand these concepts:**

* Think of STDIN as a program's ears, where it listens for user input.
* Think of STDOUT as a program's mouth, where it speaks its results.
* Think of STDERR as a separate way for the program to express error messages or warnings, without interrupting its regular output.

**Key Points:**

* STDIN, STDOUT, and STDERR are file streams, but they are typically pre-connected to the user's terminal for convenience.
* Redirection allows you to change the default behavior and send input from a file or send output/errors to a file for logging or analysis purposes.
* Many programming languages and shells provide functions or methods to work with STDIN, STDOUT, and STDERR for data input, output control, and error handling.

By understanding STDIN, STDOUT, and STDERR, you gain a deeper understanding of how programs interact with their environment and how to manage their input and output effectively.

In Bash scripting, the `set` builtin command offers various options (flags) that control how the shell behaves when executing a script. Here's a breakdown of some commonly used options:

**1. `set -x` (Trace Execution):**

* **Function:** When enabled with `set -x`, the shell prints each line of the script as it's read and about to be executed. This provides a step-by-step trace of the script's execution flow.
* **Use Case:** Use `set -x` for debugging purposes. It helps you visualize how the script progresses, identify where specific lines are being executed, and pinpoint potential issues.

**2. `set -e` (Exit on Simple Errors):**

* **Function:** With `set -e` enabled, the script exits immediately (with a non-zero exit code) if any command within the script returns a non-zero exit status (indicating an error). This helps prevent the script from continuing execution potentially causing further problems.
* **Use Case:** Use `set -e` to enforce stricter error handling. It ensures the script doesn't proceed if any command encounters an error, promoting cleaner script execution.

**3. `set -o option_name` (Set Shell Options):**

* **Function:** The `set -o` option allows you to set or unset various shell options that influence script behavior. There are numerous options available, each with its specific functionality. Here are a couple of examples:
    * `set -o verbose`: Makes the shell print expanded command lines before executing them (similar to `set -x` but for the entire command line).
    * `set -o nounset`: Makes the script exit with an error if it tries to use an undefined variable.

**Using `set` options in your script:**

You can include these options at the beginning of your script to enable them for the entire script's execution. For example:

```bash
#!/bin/bash
set -e  # Exit on errors
set -x  # Trace script execution (for debugging)

# Your script commands here
```

**Important Considerations:**

* Using `set -x` can generate a lot of output, making it less suitable for production scripts. It's primarily for debugging.
* `set -e` can be helpful to catch errors early, but consider handling specific errors gracefully if needed, instead of immediate script termination.
* Explore the various `set -o` options and their functionalities by referring to the Bash manual (`man bash`).

By understanding these `set` options, you can enhance your Bash scripting by enabling tracing, enforcing error handling, and customizing shell behavior for your specific needs.
