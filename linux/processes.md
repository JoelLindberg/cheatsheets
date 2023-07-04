# Processes

## top

Cycle through memory scales in the summary view: `shift + E` \
Cycle through memory scales in the task view: `e`

*Ranges from KiB &rarr; EiB.*

Toggle memory summary view: `m`

Add a column for thread count: `f` or `shift + F` &rarr; Move to **nTH** and hit space &rarr; `q`

*Useful for threaded software such as MySQL, Apache and Nginx etc.*

*PPID (parent proces id) might be useful for alot of cases too.*

Toggle command column: `c`

*Displays the path or command that started the process.*

CPU toggle single/multiple: `1` \
CPU toggle information shown: `t` \
Move between columns: `<` and `>` \
Update interval: `d` or `s`

Color toggle. `z` \
Changle color mapping: `shift + Z` \
Idle processes toggle: `i` \
Toggle bold in summary view: `shift + B` \
Toggle bold in task viwe: `b` \
Highlight column (affected by bold toggle): `x`\
Highlight row (affected by bold toggle): `y`

Toggle tree view: `shift + V`

Help: `h`

Save all changes: `shift + W`

*This will write you changes to the configuration file: ~/.config/procps/toprc*

Output for a specified user: `top -u <user>`

Sources: \
https://www.redhat.com/sysadmin/customize-top-command \
Kibibytes and Kilobytes: https://sv.wikipedia.org/wiki/Kibibyte

<br />

## ps

### List processes

To see every process using BSD syntax: `ps -aux`

List processes: `ps -a` \
List processes by user found in the userlist: `ps -u` \
Display processes not executed in the terminal: `ps -x`

To see every process using standard syntax (-e is the same as -A): `ps -eF`

List all processes: `ps -e` \
Full format listing: `ps -F`

## kill

Kill sends the **SIGTERM** signal.

Kill a process by PID: `kill <pid>` \
Kill a process by process name: `killall <process name>`

If you are dealing with a stuck process, add to `kill -9 <pid>` (works with killall as well). The process is reported as killed instead of terminated.

Note that **killall** will terminate all processes with the specified name.

<br />

## jobs, bg and fg

Pause/suspend and resume jobs started in your shell.

A job is a process that the shell is managing and hasn't finished running. It could be a process that is running either in the foreground or the background.

Terminate by sending SIGTSTP signal: `ctrl + z`

*Send a foreground process to a stopped state.*

Terminate by sending a SIGINT signal: `ctrl + c`

*Terminate a process that don't stop or is hung.*

**Example 1 Start**

Start a job and then stop it by entering `ctrl + z`:
~~~bash
$ sleep 300
$ ^Z
~~~

You should now be able to see the job in a stopped state using `jobs`:

~~~bash
$ jobs
[1]-  Stopped                 sleep 300
~~~

The job can be resumed by bringing it to the foreground:
~~~bash
$ fg %1
~~~

Useful options for the jobs command:

List PIDs: `jobs -l` \
List PIDs only: `jobs -p` \
List only running jobs: `jobs -r` \
List only stopped jobs: `jobs -s`

**Example 1 End**

A job can also be resumed in the background:

~~~bash
$ sleep 300
^Z
$ jobs
[1]-  Stopped                 sleep 300
$ bg %1
$ jobs
[1]+  Running                 sleep 500 &
~~~

More options for `bg`:

A job started by a command beginning with abc: `bg abc` \
A job started by a command containing abc: `bg ?abc` \
The previous job: `%--`

Start a job in the background directly:
~~~bash
$ sleep 400 &
[2]-  Running                 sleep 400 &
~~~

Sources:
* https://www.redhat.com/sysadmin/jobs-bg-fg
* https://chrisjean.com/multitasking-from-the-linux-command-line-plus-process-prioritization/

