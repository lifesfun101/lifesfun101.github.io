---
layout: post
title: "Pentest Primer: Learn Best Linux Commands for Beginners Now"
date: 2025-02-19
---

{% include youtube.html id="gT_m7Vnli-w" %}

This lesson introduces essential Linux commands and scripting fundamentals crucial for penetration testing and cybersecurity work.

Scripting refers to the creation and execution of sequences of commands or instructions to perform specific tasks. In penetration testing, scripting empowers testers to automate various procedures, interact with systems, and analyze data efficiently. It encompasses a wide array of languages and tools tailored to address different aspects of security testing.

Why Scripting Still Matters in the AI Era
---
With the rise of AI bots and assistants, you may wonder why learning scripting is still necessary. However, AI is still far from perfect, and you still need to understand what AI is writing for you. Scripting remains important for several critical reasons:

**Understanding and Verification**: You must be able to read and verify AI-generated code for correctness and security. Blindly running code you don't understand is dangerous in security contexts.

**Customization**: AI can provide templates, but you need scripting knowledge to customize solutions for specific penetration testing scenarios.

**Debugging**: When scripts fail or produce unexpected results, you need fundamental understanding to troubleshoot effectively.

**Speed and Efficiency**: Experienced practitioners can often write simple scripts faster than describing requirements to an AI and validating the output.

**Professional Competency**: Security professionals are expected to have scripting skills. It's a fundamental competency in the field.

Scripting Languages for Penetration Testing
---
This series covers three essential scripting languages for penetration testing:

**Bash**: Linux shell scripting for automation, system interaction, and command chaining. Essential for working in Linux environments and automating reconnaissance tasks.

**Python**: Versatile, high-level language perfect for tool development, data analysis, and complex automation. Most popular language for security tool development.

**PowerShell**: Windows scripting and automation language. Essential for testing and exploiting Windows environments, Active Directory attacks, and post-exploitation activities.

Essential Linux File Management Commands
---
Before diving into scripting, mastering basic Linux commands is crucial. These commands form the foundation for navigation, file management, and system interaction.

**Navigation and Information**:

`pwd` (Print Working Directory): Displays your current location in the filesystem
- Usage: `pwd`
- Shows the full path of your current directory

`ls` (List): Displays contents of directories
- Usage: `ls [options] [directory]`
- Common options:
  - `ls -l`: Long format with permissions, owner, size, date
  - `ls -a`: Show all files including hidden files (starting with .)
  - `ls -lh`: Long format with human-readable file sizes
  - `ls -R`: Recursively list subdirectories
- Example: `ls -lah` shows all files in long format with human-readable sizes

`cd` (Change Directory): Navigate between directories
- Usage: `cd [directory]`
- Special directories:
  - `cd ~`: Go to home directory
  - `cd ..`: Go up one directory level
  - `cd -`: Return to previous directory
  - `cd /`: Go to root directory

**File and Directory Operations**:

`mkdir` (Make Directory): Create new directories
- Usage: `mkdir [options] directory_name`
- Options:
  - `mkdir -p path/to/directory`: Create parent directories as needed
  - `mkdir -m 755 directory`: Create with specific permissions

`rm` (Remove): Delete files and directories
- Usage: `rm [options] file_or_directory`
- Options:
  - `rm -r directory`: Remove directory recursively
  - `rm -f file`: Force removal without confirmation
  - `rm -rf directory`: Force recursive removal (use with extreme caution!)
- Warning: rm is permanent - there's no recycle bin

`cp` (Copy): Copy files and directories
- Usage: `cp [options] source destination`
- Options:
  - `cp -r source_dir dest_dir`: Copy directory recursively
  - `cp -p file dest`: Preserve file attributes
  - `cp -i file dest`: Prompt before overwriting

`mv` (Move): Move or rename files and directories
- Usage: `mv [options] source destination`
- Examples:
  - `mv oldname newname`: Rename file
  - `mv file /new/location/`: Move file to new location
  - `mv -i file dest`: Prompt before overwriting

**File Viewing and Manipulation**:

`cat` (Concatenate): Display file contents
- Usage: `cat file1 [file2 ...]`
- Can concatenate multiple files
- Example: `cat file1 file2 > combined`: Combine files

`echo`: Display messages or output text
- Usage: `echo "message"`
- Common uses:
  - `echo $VARIABLE`: Display variable value
  - `echo "text" > file`: Write to file (overwrite)
  - `echo "text" >> file`: Append to file

`grep`: Search for patterns in files
- Usage: `grep [options] pattern [files]`
- Options:
  - `grep -i pattern file`: Case-insensitive search
  - `grep -r pattern directory`: Recursive search
  - `grep -v pattern file`: Invert match (show non-matching lines)
  - `grep -n pattern file`: Show line numbers
  - `grep -E pattern file`: Use extended regex
- Essential for log analysis and finding specific content

**Advanced Text Processing**:

`cut`: Extract specific fields from delimited text
- Usage: `cut [options] file`
- Examples:
  - `cut -d':' -f1 /etc/passwd`: Extract usernames
  - `cut -c1-10 file`: Extract characters 1-10

`tr` (Translate): Character-level substitution
- Usage: `tr [options] set1 set2`
- Examples:
  - `tr 'a-z' 'A-Z'`: Convert lowercase to uppercase
  - `tr -d ','`: Delete all commas

`sed` (Stream Editor): Text manipulation and search-replace
- Usage: `sed [options] 'command' file`
- Examples:
  - `sed 's/old/new/g' file`: Replace all occurrences
  - `sed '/pattern/d' file`: Delete lines matching pattern
  - `sed -n '1,10p' file`: Print lines 1-10

`awk`: Data extraction and reporting
- Usage: `awk 'pattern {action}' file`
- Examples:
  - `awk '{print $1}' file`: Print first field
  - `awk -F':' '{print $1}' /etc/passwd`: Print usernames
  - `awk '$3 > 100' file`: Print lines where 3rd field > 100

File Permissions and Ownership
---
Understanding and managing file permissions is crucial for both security testing and system administration.

`chmod` (Change Mode): Modify file permissions
- Usage: `chmod [options] mode file`
- Numeric mode:
  - Read (r) = 4, Write (w) = 2, Execute (x) = 1
  - `chmod 755 file`: rwxr-xr-x (owner: rwx, group: rx, others: rx)
  - `chmod 644 file`: rw-r--r-- (owner: rw, group: r, others: r)
  - `chmod 700 file`: rwx------ (owner only)
- Symbolic mode:
  - `chmod u+x file`: Add execute for user
  - `chmod g-w file`: Remove write for group
  - `chmod o=r file`: Set others to read-only

`chown` (Change Owner): Modify file ownership
- Usage: `chown [options] user:group file`
- Examples:
  - `chown user file`: Change owner
  - `chown user:group file`: Change owner and group
  - `chown -R user:group directory`: Recursive change

Process Management
---
Managing processes is essential for understanding system activity and controlling running applications.

`ps` (Process Status): Display running processes
- Usage: `ps [options]`
- Common options:
  - `ps aux`: Show all processes for all users
  - `ps -ef`: Full-format listing
  - `ps -u username`: Show processes for specific user

`kill`: Terminate processes
- Usage: `kill [signal] PID`
- Common signals:
  - `kill PID`: Graceful termination (SIGTERM)
  - `kill -9 PID`: Force kill (SIGKILL)
  - `kill -HUP PID`: Reload configuration (SIGHUP)

File Search
---
`find`: Search for files and directories
- Usage: `find [path] [conditions]`
- Examples:
  - `find / -name "*.log"`: Find all .log files
  - `find /home -user username`: Find files owned by user
  - `find / -perm 777`: Find files with 777 permissions
  - `find / -type f -size +100M`: Find files larger than 100MB
  - `find / -mtime -7`: Find files modified in last 7 days

Hands-On Practice Lab
---
To practice these commands, create a lab environment with this Bash script:

```bash
mkdir -p ~/bash_lab_1/file_management
touch ~/bash_lab_1/file_management/example{1,2,3}.log
touch ~/bash_lab_1/file_management/conf{1,2,3}.yml
touch ~/bash_lab_1/file_management/.hidden{1,2,3}
sudo cp /var/log/boot.log ~/bash_lab_1/file_management/boot.log
```

This creates:
- A directory structure for practicing
- Multiple log files to work with
- Configuration files
- Hidden files to discover
- A real system log to analyze

Practical Exercises:

1. Navigate to the lab directory and list all files including hidden ones
2. Search for specific patterns in log files using grep
3. Use find to locate all .log files
4. Change permissions on files using chmod
5. Practice text manipulation with sed and awk
6. Extract specific information from log files using cut and grep

Applying Commands to Penetration Testing
---
These commands are essential throughout penetration testing:

**Reconnaissance**: Using grep and awk to parse scan results, find to locate interesting files, ps to identify running services

**Enumeration**: Combining commands with pipes to filter and analyze data efficiently

**Log Analysis**: Using grep, sed, and awk to analyze system logs and identify security events

**Privilege Escalation**: Finding SUID binaries, world-writable files, and misconfigurations

**Post-Exploitation**: Navigating systems, finding sensitive data, understanding system configuration

Building Scripting Skills
---
After mastering individual commands, the next step is combining them into scripts:

1. Start with simple one-liners combining commands with pipes
2. Save useful command combinations as shell scripts
3. Learn basic Bash scripting (variables, loops, conditionals)
4. Progress to Python for more complex automation
5. Add PowerShell for Windows environments

Remember: The goal isn't memorizing every option for every command. Focus on understanding core functionality and knowing how to look up syntax when needed (`man command` or `command --help`).

The commands covered in this lesson form the foundation for all Linux-based penetration testing activities. Mastery of these fundamentals is essential before progressing to more advanced techniques and tools.
