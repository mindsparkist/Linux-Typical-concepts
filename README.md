# Linux-Typical-concepts
https://shuvradipchakrabortyportfolio.blogspot.com/2022/10/linux-notes.html

# Linux Concept

## Switching users in a multiuser enviorment 

In a multiuser enviourment shell could me divided into 2 major parts 
1 login shell 
2 intactive shell

login shell 
- A login shell is a shell session initiated after a user logs into the system, either locally or remotely.
- Login shells typically execute scripts like .bash_profile, .bash_login, or .profile depending on the shell (e.g., Bash, Zsh) to set up the user environment.
- which load up lib, paths
- to login in this mode use command >su -

interactive shell 
- An interactive shell is a shell session where the user can interactively issue commands and receive responses.
- It's not necessarily tied to a login; you can open an interactive shell within a terminal emulator without having to log in again.
- it usually loads .bashrc file
- to login in this mode use command >su 


Run levels in Linux are different operating states defined by numbers, typically ranging from 0 to 6. Each run level specifies which system services and processes are running, essentially controlling what your system can do in that state. 

Here's a breakdown of the concept:

**Function:**

* Run levels determine the overall functionality of your Linux system after booting.
* They act like presets, configuring the system for specific purposes.

**Use Cases:**

* **Single-user mode (run level 1):** Ideal for system maintenance and troubleshooting, allowing only one user with limited access.
* **Multi-user mode (run levels 2, 3, 4):** Provide varying configurations for multi-user environments, with or without networking, depending on the specific run level.
* **Graphical user interface (GUI) mode (run level 5):** The standard desktop environment for everyday use, with most services running.
* **Reboot (run level 6):** Initiates a system restart.
* **Shutdown (run level 0):** Halts the system completely.

**Importance:**

* Run levels offer control over resource usage and system behavior.
* They enable administrators to tailor the system to specific needs, like running a server without a GUI for efficiency.
* https://www.youtube.com/watch?v=LPu3IZwbOA8 -- check for runlevel

**Modern Usage:**

While still present in traditional Linux distributions, the use of run levels has somewhat diminished in recent times. 

* Systemd, a widely adopted init system (the process responsible for system initialization), often utilizes different mechanisms for service management.
* Many distributions now default to a single run level with the ability to start or stop specific services as needed, offering more flexibility.

Therefore, while understanding run levels remains valuable for general Linux knowledge, their practical application may vary depending on the specific distribution and its chosen init system.



## systemctl: The Control Panel for Your Linux System

The `systemctl` command is a powerful tool in Linux for managing system services. It interacts with `systemd`, the init system responsible for starting, stopping, and managing services at boot and runtime. 

Here's a breakdown of what `systemctl` can do:

**Main functionalities:**

* **Start a service:** Use `sudo systemctl start <service_name>` to initiate a service.
* **Stop a service:** Use `sudo systemctl stop <service_name>` to halt a running service.
* **Restart a service:** Use `sudo systemctl restart <service_name>` to stop and then start a service again.
* **Check status:** Use `sudo systemctl status <service_name>` to view details about a service, including its current state, active PID (process ID), and recent logs.
* **Enable/disable services:**
    * `sudo systemctl enable <service_name>`: Allows the service to start automatically at boot.
    * `sudo systemctl disable <service_name>`: Prevents the service from starting automatically.
* **List services:** Use `sudo systemctl list-units` to see a list of all available services, including their status and enabled/disabled state.

**Additional features:**

* `systemctl` offers various subcommands for advanced tasks like reloading service configurations, checking failed services, and managing unit files (which define service behavior).

**Benefits:**

* **Unified management:** `systemctl` provides a central platform to manage all system services, simplifying administration.
* **Clear status:** It offers detailed information about service states and logs, aiding troubleshooting.
* **Flexibility:** You can control how services start, stop, and behave under various conditions.

**Important points:**

* **Root privileges:** Most `systemctl` operations require root access, so use `sudo` before the command.
* **Service names:** Replace `<service_name>` with the actual service you want to manage. Common examples include `sshd.service` for SSH or `apache2.service` for the Apache web server.

**Learning resources:**

* To delve deeper, refer to the official systemd documentation: [https://man7.org/linux/man-pages/man1/systemd.1.html](https://man7.org/linux/man-pages/man1/systemd.1.html)
* Many online tutorials offer practical examples and explanations: [https://www.geeksforgeeks.org/systemctl-in-unix/](https://www.geeksforgeeks.org/systemctl-in-unix/)

I hope this explanation empowers you to use `systemctl` effectively for managing your Linux system!

## Targets vs. Runlevels: Modern Linux Service Management

Both targets and runlevels are concepts related to managing services at boot and runtime in Linux. However, they represent different approaches, with targets being the more modern way.

**Runlevels:**

* **Traditional Approach:** Runlevels are a historical method for defining different system states using numbered labels (typically 0-6).
* **Functionality:** Each runlevel specifies a set of services to start or stop, resulting in varying system functionalities.
    * Example: Runlevel 3 might be a multi-user mode with networking, while runlevel 5 is the graphical user interface (GUI) mode.
* **Limitations:** Runlevels offer a limited number of predefined states, making them less flexible for complex system configurations.
* **Modern Usage:** While still present in some distributions, their use has diminished with the rise of systemd.

**Targets:**

* **Modern Approach:**  Targets are more descriptive names representing specific system states managed by systemd (the init system in most modern distributions).
* **Functionality:** Similar to runlevels, targets define which services are active, but they offer more flexibility and can be dynamically created or modified.
* **Benefits:**
    * **Descriptive Names:** Targets have clear names like "multi-user.target" or "graphical.target" for better understanding.
    * **Flexibility:**  You can create custom targets with specific service configurations for unique needs.
    * **Dependency Management:** systemd handles service dependencies efficiently within targets.
* **Relationship to Runlevels:**
    * Systemd often maps traditional runlevels to specific targets for backward compatibility.
    * You might find files named `runlevel.target` which link to actual systemd targets.

Here's a table summarizing the key differences:

| Feature       | Runlevels                   | Targets                      |
|---------------|----------------------------|-------------------------------|
| Approach      | Traditional                | Modern                        |
| Definition    | Numbered labels (0-6)        | Descriptive names             |
| Flexibility   | Limited                    | More flexible, customizable  |
| Dependency Mgmt | Less sophisticated          | Efficient handling by systemd  |
| Modern Usage  | Diminished                  | Dominant approach with systemd  |


In essence, targets are the successor to runlevels, offering a more powerful and adaptable way to manage services in contemporary Linux systems.

## Booting into Different Targets Manually in Linux with systemd

Systemd, the init system in most modern Linux distributions, manages various boot targets that define different system states.  Here's how you can boot into different targets manually and explore the relationship between them:

**Common Systemd Targets:**

* **multi-user.target:** This is a basic multi-user environment that allows multiple users to log in via text-based terminals. Networking is typically enabled in this target.
* **graphical.target:** This target boots the graphical user interface (GUI) environment, providing the standard desktop experience.
* **rescue.target:** This target launches a minimal single-user environment for system recovery purposes. It allows root access to perform repairs on a non-functional system.
* **poweroff.target:** Initiates a system shutdown.
* **reboot.target:** Initiates a system restart.

These are just a few common examples, and your specific distribution might have additional targets for specialized purposes.

**Booting into a Specific Target:**

There are two main ways to boot into a different target manually:

**1. Using the GRUB Bootloader:**

* During system boot, when the GRUB menu appears (usually with a short timeout), press `e` to edit the boot options.
* Find the line that starts with `linux /boot/vmlinuz-… root=… ro quiet splash` (the exact format might vary).
* Add `systemd.unit=desired_target.target` (replace `desired_target.target` with the actual target name, e.g., `multi-user.target`) at the end of the line.
* Press `Ctrl+X` or `F10` to boot with the modified options.

**2. Using systemctl (systemd command):**

* (**Caution:** This method requires root privileges)
* Boot your system normally into the default target.
* Once logged in, use the following command to switch to a different target:

```bash
sudo systemctl isolate desired_target.target
```

* Replace `desired_target.target` with the target you want to switch to (e.g., `sudo systemctl isolate multi-user.target`).

**Target Dependencies:**

Systemd targets can have dependencies on other targets or services. This ensures that essential services required for a particular target are started first. For example, the `graphical.target` might depend on the `multi-user.target` because the GUI needs a basic multi-user environment to function.

**How to Check Dependencies:**

You can use the following command to view the dependencies of a specific target:

```bash
systemctl list-units desired_target.target --full
```

This will display information about the target, including its dependencies and their status.

**Important Notes:**

* Booting into a target like `rescue.target` might require a root password for access.
* Modifying boot options with GRUB is temporary and only affects the current boot.
* Experimenting with different targets is a great way to learn about systemd and how it manages services. However, proceed with caution, especially when using `systemctl isolate` as it can leave your system in a non-standard state. It's recommended to reboot after exploring a different target.


By understanding these concepts and methods, you can gain more control over your Linux boot process and explore different system states for troubleshooting or specific needs.


## Linux Deamon 

In Linux, a daemon (pronounced like "day-mon") is a special type of program that runs in the background, autonomously performing tasks without requiring direct user interaction.  They are the silent workhorses of the system, ensuring essential services function smoothly.

Here's a closer look at what daemons do:

* **Background Processes:** Unlike programs you launch from the terminal and interact with, daemons typically run in the background, unseen by the user.  They don't require a terminal window to be open and continue functioning even after you log out.
* **Service Management:** Daemons are responsible for managing various system services,  like:
    * **sshd:** This daemon enables secure remote login access to your system.
    * **httpd (Apache):**  This daemon manages web server functionality if you're running a website.
    * **cron:** This daemon runs scheduled tasks and programs at predefined times.
* **Silent Operation:** Daemons typically operate silently,  meaning they don't provide any visual cues or interact with the user interface.  However, you can often check their status or logs using system tools like `systemctl`.
* **Automatic Start/Stop:** Most daemons are configured to start automatically during system boot and continue running until the system is shut down.  You can also manage their startup and shutdown behavior using tools like `systemctl`.

Here are some key characteristics of daemons:

* **Long-running processes:** They are designed to be persistent and can stay active for extended periods,  sometimes for the entire uptime of the system.
* **Low resource usage:** Ideally, daemons are written to be efficient and consume minimal system resources like CPU and memory.

In summary, daemons are essential components of a Linux system,  running critical services behind the scenes to keep everything functioning properly. They provide a robust and automated way to manage various tasks, ensuring a smooth user experience.


In Linux, SUID (Set User ID), SGID (Set Group ID), and the Sticky bit are special permissions that extend beyond standard read, write, and execute permissions for files and directories. They can be powerful tools for granting temporary privileges or controlling access, but also require caution due to potential security risks if not used properly.

Here's a breakdown of each:

**1. SUID (Set User ID):**

* **Function:** Allows anyone who executes the file to temporarily gain the permissions of the file's owner. 
* **Use Case:** Used for programs that need to perform actions that normally require root privileges (e.g., `sudo`). A classic example is the `passwd` command, which lets regular users change their own password but requires root access to modify others'.
* **Security Risk:** If a SUID program has a vulnerability, it could be exploited by an attacker to gain unauthorized root access. Use SUID with caution and only on essential programs.

**2. SGID (Set Group ID):**

* **Function:** When a file with SGID is executed, it runs with the effective group ownership of the file, not necessarily the group of the user who executed it.
* **Use Case:** Useful for allowing members of a specific group to access or modify files that they wouldn't normally have permission to change. 
* **Example:** A directory with SGID set might allow any member of the "webserver" group to add or remove files within that directory, useful for collaborative tasks.

**3. Sticky Bit (Sticky bit on directories):**

* **Function:**  Applies only to directories. When set, only the owner of a file within the directory or the root user can delete or rename that file. This prevents other users, even if they have write permissions for the directory, from modifying or deleting files they don't own.
* **Use Case:** Useful for directories like `/tmp` where multiple users write temporary files. The sticky bit ensures users can only delete their own temporary files and not those belonging to others. 

**How to Set and Check these Permissions:**

Here's how you can create a process, send it to the background, stop it, and start it again in RHEL:

**1. Creating a Process:**

Let's use the `sleep` command as an example process. This command simply pauses execution for a specified time. You can replace it with any other command you want to run.

```bash
# This command sleeps for 10 seconds
sleep 10
```

**2. Sending the Process to the Background:**

There are two ways to send the process to the background:

* **Using the `&` symbol:** Append an ampersand (`&`) symbol to the end of the command when you run it. This tells the shell to run the command in the background and return control to you immediately.

```bash
sleep 10 &
```

* **Using `bg` after starting the process:** If you forget the `&` symbol initially, you can use the `bg` command to send an already running process in the foreground to the background.

```bash
sleep 10
# Press Ctrl+Z to suspend the process
bg
```

**3. Checking Background Jobs:**

You can use the `jobs` command to view a list of your background jobs. This will display the job ID and the command associated with it.

```bash
jobs
```

**4. Stopping the Process:**

To stop a background process, you can use the `kill` command followed by the job ID obtained from the `jobs` command.

```bash
jobs
# Output will be similar to:
# [1] (running) sleep 10 &

# Get the job ID (in this case, 1)
kill %1
```

**5. Starting the Process Again:**

There are two ways to start a background process again, depending on the command used to stop it:

* **If stopped with `Ctrl+Z`:** Use the `fg` command followed by the job ID to bring the process back to the foreground and resume execution.

```bash
jobs
# Get the job ID (e.g., 1)
fg %1
```

* **If stopped with `kill`:** Unfortunately, `kill` typically terminates the process completely.  You'll need to restart the process by running the original command again.

```bash
# This will start a new instance of the sleep command
sleep 10 &
```

**Remember:**

* Be cautious when using `kill` as it can lead to data loss for some processes.
* Make sure to replace `sleep 10` with the actual command you want to run in the background.

This approach allows you to manage background processes effectively in RHEL. 

* You can use the `chmod` command with specific flags to set or modify these permissions. 
* For example, `chmod u+s filename` sets SUID on a file,  and `ls -l` command with detailed output format displays these special permissions along with standard read, write, and execute permissions.

Remember, using these permissions incorrectly can create security vulnerabilities. Make sure you understand the implications before applying them, and only use them on programs or directories that genuinely require these special access controls.

