# Issue #1
## Article one - Privilege escalation
### Crossroads
In windows the OS parses execute commands in the following way:
`
C:\path\to\file argument argument etc....
`

Path to Executable|argument|argument
---|---|---
"C:\path\to\file"|"argument"|argument"

but if the path doesn't exit windows parses the command the following way

Path to Executable|argument
---|---
"C:\path\to\file argument"|"argument"

and if this doesn't exit windows parses the command the following way

|Path to Executable
|---
|"C:\path\to\file argument argument"

THe way to use this behavior is to find a program that is defaultly executed with high privileges with a vulnerable path (path with spaces) and lace you own executable i.e:

default program path|my program path my executable
---|---
C:\Program Files\Admin.exe|C:\Program

This way you can run an arbitrary program with admin privileges,
in the case above windows will run the program you placed in "C:\Program" with the argument "Files\Admin.exe"
`
"C:\Program" "Files\Admin.exe"
`
I want to note that we need to add an extension to C:\Program because of windows. the Extensions we can add are all the extension that we get if we run `echo %PATHEXT%`, So a proper crossroads would be to place our file in C:\ with the name Program.exe

#### Summery
The thing to take from here is to look at how the OS parses commands, parsing is always vulnerable...

### At.exe (old version of windows task scheduler)
At is running with system privileges as it should be able to access system data, for us it means that if we can get a shell run by At we will have system privileges as the shell will inherit At's privileges.
To do so the only thing we need is to give At a task to run a shell interactively. We can even run an explorer and close our Explorer.exe and this way we can basically get into an system user shoes so easily its even funny.
Of course this was patched in sp2 of vista but its interesting as a concept.

#### Summery
Any program that we can use to run other programs could be a good way to escalate. Schedulers in particular are an interesting target.
