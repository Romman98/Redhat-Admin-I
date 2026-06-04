# Learning the Fundamentals

## The Shell 🐚

### Shell 

A `shell` in linux is the interpreter that allows you to interact with the OS. The way commands are interpreted is determined by variables and paramteters associated with each shell. The default shell in linux is `bash`, which stands for "Bourne Again Shell". 

### GUI & CLI
You can access the shell from the `GUI shell interface` then access the terminal. You cn also  open a `virtual CLI` either by connecting a console cable or through SSH.

### Command Syntax

The general syntax of a linux command is
```
COMMAND [OPTIONS] [ARGUMENTS] 
```

>COMMAND --> The program to be executed. \
OPTIONS --> Modify the command behviour \
ARGUMENTS --> The object the command acts

```
# Example

ls -l /home/admin
```
>ls    -- Command \
-l    -- Option \
/home -- argument

#### 1. Traditional Style
Historically, some UNIX commands accepted options without a leading dash.
```
tar xvf backup.tar
```

#### 2. UNIX Style
Short options begin with a single dash - \
Options can be combined together
``` 
ls -l -a
ls -la 
```

#### 3. GNU Style
Long options use a double dash --.

```
ls --all
```
Long options improve readability and self-documentation.

Not all commands follow all three styles. Command syntax depends on the command implementation.

### Command Execution

When you press **Enter**, the shell performs several steps to execute the command.

```text
User
 ↓
Shell reads and parses the command
 ↓
Shell determines the command type
(alias → builtin → executable)
 ↓
Shell searches the command location using PATH
(if needed)
 ↓
Shell creates a new process
 ↓
Kernel loads and runs the program
 ↓
Program executes and produces output
 ↓
Shell displays output and waits for the next command
```

#### Example

```bash
ls -l /home
```

Execution flow:

1. The shell receives `ls -l /home`
2. The shell identifies `ls` as an executable command
3. The shell searches directories listed in `$PATH`
4. The shell creates a new process
5. The kernel loads and runs `/usr/bin/ls`
6. The program executes and generates output
7. The shell prints the output
8. The shell becomes ready for the next command

---

#### Notes

Not every command launches a new executable.

Example:

```bash
cd /tmp
```

`cd` is a **shell builtin**, meaning the shell executes it directly without searching `$PATH`.

You can check command types using:

```bash
type cd
type ls
```

Example output:

```text
cd is a shell builtin
ls is /usr/bin/ls
```

#### Key Takeaways

- The shell interprets and manages command execution.
- The kernel manages process creation and execution.
- Commands can be aliases, builtins, or executables.
- `$PATH` is used to locate executable commands.


## Terminal Commands 

- which
- type
- whoami
- who
- last
- man
- pwd
- ls

