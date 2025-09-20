# A Primer on pwntools

## Introduction

When developing exploits for various challenges, it might be beneficial to use scripts and automation to make your life easier. Especially when you want to deal with sending bytes and data that are not printable, or large numbers when dealing with cryptography.

Of course, one of the easiest scripting languages to use is Python. While you could manually set up connections, text piping, and the whole host of annoying debugging issues that come along with that, there's already a handy library to handle the plumbing for you.

`pwntools` is a Python library with *lots* of handy functions, classes, and scripts to help automate and streamline exploit development.

You can install pwntools through `pip`, Python's package manager:

```
pip3 install pwntools
```
or
```
python3 -m pip install pwntools
```

### Mac/Linux pwntools installation help
If the above command does not work for you on MacOS or Linux, you may need some dependencies that are required to install pwntools. Specifically, pwntools expects you to have `cmake` and `pkg-config`. Try running: 
```
brew install cmake
brew install pkg-config
```
or 
```
sudo apt-get install cmake
sudo apt-get install pkg-config
```

## Interacting with a process

`pwntools` uses the idea of "tubes" to handle data transfer/receive. You can make a connection with an actual network interface (like you would with `netcat`), or with a local process, and link standard in and standard out with `pwntools`.

Here's a few ways to make a connection:
```py
from pwn import * # It's usually just better to import everything when using pwntools

# Start a process on your machine, locally
conn = process('./file')

# Start a process and simultaneously start gdb debugging it.
# You can provide an initial set of commands using the gdbscript parameter
conn2 = gdb.debug('./file', gdbscript="""
b main
continue
""")

# Pause a process running locally and attach gdb to it and start debugging
gdb.attach(conn)

# Connect to a server
port = 22
conn3 = remote('ip.or.domain', port)
```

With `process`, it just launches the file specified via the path. If you give it an array of strings, you can run a file with command line arguments, e.g. `['./file', '-t', 'data.txt']`. You can also start a program locally with a debugger using `gdb.debug()` instead. **You need to be using a terminal with support for paneling**. So install and enter terminal multiplexer such as `tmux` before running a debugger with pwntools (Windows Terminal, oddly, supports this natively?). You can also attach gdb to an existing process with `gdb.attach()`. Finally, for communicating with networks (and in our case, challenge servers), you can use `remote()` to provide an ip/domain and port to connect to.

Once we have our connection, we can send or receive data from it. There are many functions to do this, but I'll show off a few:

```py
from pwn import *
port = 9999
conn = remote('chal.sigpwny.com',port)

# Receive functions will return a bytestring of what it receives.

# Receive text until it encounters a new line character (e.g. \n)
line = conn.recvline()

# Receive text until it encounters the specified delimiter.
prompt = conn.recvuntil(b'>> ')

# Receive exactly n bytes.
resp = conn.recvn(8)

# Receive everything until End of File (EOF) is received from the pipe (i.e. they closed the connection)
everything = conn.recvall()

# Sends the string along with an new line ending
conn.sendline(b'my response\x05\x0f\xa0')

# Does NOT send a newline
conn.send(b'not a line yet')

# Does a recvuntil('delim') and sends the 2nd argument
conn.sendlineafter(b'>> ', b'1')
```

Note that strings in these functions are bytestrings `b''`. pwntools will warn you if you dont use bytestrings, and automatically convert to ASCII.
That being said, this means its really easy to send non-printable characters over the wire using `\x` escape. It's also important to keep in mind that functions looking for a specific character (like a new line or delimiter), will block until it does. If your script is stuck, check to see whether or not you are actually getting the text you think you are. *Running the script with the DEBUG argument will cause it to print a hex dump of any data it sends/receives*: `python3 exploit.py DEBUG`

Note, pipes with processes and such are *buffered*. The OS maintains a 4kb buffer of any text received. If you call `recvline()`, and the program sent 3 lines in total, only the first line is retrieved, and the other 2 remain in the buffer. Keep this in mind when processing received data. If you want to retrieve anything that's in the buffer currently, `recv()` will empty it out.

Finally, if you want to regain control of the process input/output, simply call:
```py
conn.interactive()
```
You'll want this to read the final printout of a program after running your exploit, or to use a shell, etc.

## Using shellcode
For some of our challenges, you will need to use `pwntool`s `asm` function. This requires a binutils installation for the architecture that our machines are running on, in this case, amd64. If you are on an M1/M2 mac, you'll need to run the following commands somewhere to generate assembly for our servers:
```
wget https://raw.githubusercontent.com/Gallopsled/pwntools-binutils/master/macos/binutils-amd64.rb
brew install binutils-amd64.rb
```