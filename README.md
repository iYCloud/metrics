# metrics
metrics

# Problem
As a DevOps engineer, you need to collect metrics about your Linux server. Unfortunately, you cannot use any monitoring solution and need to implement something on your own. Implement a script which prints basic information about your OS to the console.

You may use any language of your choice to implement the script.
The script should accept a single parameter to specify which metrics set to print:
1) cpu - prints CPU metrics
2) mem - prints RAM metrics

# Solution
Some kind of IDE to write and run the code with Python. The script is based on reading information from the `/proc/` directory.

## Requirments
1) Linux server/machine
2) Python must be able to import `sys`

## General script layout
Script has been separated into 6 sections to ease readability.

1) config section
2) import section
3) CPU section
4) MEM section
5) help section
6) unmarked - script logic

### Config section
Config section is meant for user interactions to append monitoring sections and values.
To see available monitoring values user must run the following command in the Linux environment.
```bash
$ cat /proc/meminfo | awk {'print $1'}
```
Output:
```bash
MemTotal:
MemFree:
MemAvailable:
Buffers:
Cached:
SwapCached:
...
```
A new list with variable(s) can be made
```python
insertVariableName = [
        "insert Trackable Value From the previous command with ':' symbol if present",
        "add another value"
]
```
If list is made, append it to the end of `memList` with a friendly name
```python
memList = [
        (virtualMemory, "virtualMemory"),
        (swap, "swap"),
        (insertVariableName, "friendly name")
]
```

### Import section
By default, the script has only one import `sys` which is necessary for reading input from the command line.

### CPU section
Information is gathered from:
1) `/proc/stat` - used to get the number of cores, summarized times and per core times.
2) `/proc/loadavg` - used to get average loads, which are presented as 1 min, 5 min and 15 min interval sections.

### MEM section
Information is gathered from one source:
1) `/proc/meminfo`

### Help section
Has just print commands in case the input parameter is missing or invalid.

### Unamraked - script logic
Simple logic based on if-else. Depending on the condition the script will go to mentioned sections 3 to 5.
3) CPU section
4) MEM section
5) help section

# Sample outputs

## No parameter
The program will give directions about the input parameter.
```bash
$ ./metrics
```
Output:

```bash
metrics: error: the following arguments are required: metric
For mem run command:
./metrics mem
For cpu run command:
./metrics cpu
```

## CPU parameter
Gives information about core count, times and load.
```bash
$ ./metrics cpu
```
Output:
```bash
cpu

cores 12

Total times
cpu      |user     |system   |idle     |iowait   |irq      |softirq  |steal    |guest    |guest_nice
cpu      |0.02     |0.0      |0.04     |99.94    |0.0      |0.0      |0.0      |0.0      |0.0

Times per core
cpu      |user     |system   |idle     |iowait   |irq      |softirq  |steal    |guest    |guest_nice
cpu0     |0.02     |0.0      |0.03     |99.94    |0.0      |0.0      |0.01     |0.0      |0.0
cpu1     |0.01     |0.0      |0.01     |99.98    |0.0      |0.0      |0.0      |0.0      |0.0
cpu2     |0.02     |0.0      |0.1      |99.87    |0.0      |0.0      |0.0      |0.0      |0.0
cpu3     |0.01     |0.0      |0.02     |99.97    |0.0      |0.0      |0.0      |0.0      |0.0
cpu4     |0.02     |0.0      |0.09     |99.89    |0.0      |0.0      |0.0      |0.0      |0.0
cpu5     |0.01     |0.0      |0.01     |99.98    |0.0      |0.0      |0.0      |0.0      |0.0
cpu6     |0.02     |0.0      |0.03     |99.95    |0.0      |0.0      |0.0      |0.0      |0.0
cpu7     |0.01     |0.0      |0.01     |99.98    |0.0      |0.0      |0.0      |0.0      |0.0
cpu8     |0.02     |0.0      |0.03     |99.95    |0.0      |0.0      |0.0      |0.0      |0.0
cpu9     |0.01     |0.0      |0.01     |99.98    |0.0      |0.0      |0.0      |0.0      |0.0
cpu10    |0.02     |0.0      |0.16     |99.82    |0.0      |0.0      |0.0      |0.0      |0.0
cpu11    |0.01     |0.0      |0.01     |99.98    |0.0      |0.0      |0.0      |0.0      |0.0

Load average over
1 min    |5 min    |15 min
0.01     |0.02     |0.00
```

## MEM parameter
Gives information about memory usage. 
```bash
$ ./metrics mem
```
Output:
```bash
virtualMemory
MemTotal: 12912808
MemFree: 8840708
swap
SwapTotal: 4194304
SwapFree: 4194304
```
The default setup only has two sections `virtualMemory` and `swap` but more sections and monitoring metrics can be added.
