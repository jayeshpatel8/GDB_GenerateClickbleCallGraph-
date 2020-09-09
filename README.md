# gen_graph.py

## How to Use:
    Interactive Execution Call Graphs:
      - Click on a node and entire parent and child chain is highlighted with different colors
      - Look at edge label to find out when it was called first and last time
      - Click on edge label to change its Color

## Description
  Python command line tool to visualize function-call-flow for a C/C++ program using graphviz dot object and matplotlib.
  Base version of script taken from https://raw.githubusercontent.com/tarun27sh/gdb_graphs (Many Thanks to Tarun27sh) and 
  modified it to enhance use exeperience for run time call flow for certain usecases
#### Modifications on base version:
     - Added sequence number to each edge to identify when first time and last time that funciton was called in sequence
       Example: (12  200) first time sequence number = 12, last time  seq. number = 200
     - Each edge Label made it Clickble to change its color and arrow color to identify easily it crowded cases.

## Motivation
To view call-graph for certain run time use cases (C++,g++,gdb,cygwin) using gdb
This helps in getting a bigger picture of the source code and makes understanding large code bases QUICK.

## Input
Requires data from gdb to get nodes (functions) and edges (function 1 calling function 2).

## Output
Di-graph showing relationship between functions across software layers, the "bigger picture".


# Usage 

There are two parts to the whole process:
1. get data by runnning process under gdb
2. process this data with gen_graph.py


## Part I: Get data from GDB

### Enable debugging symbols    
    $ test -g test.cpp

### Now, the GDB part
#### start binary under GDB     
    $ gdb a.out
    . . .
    Reading symbols from a.out...done.

#### Insert breakpoints on functions of interest, or to put in every funtion in a file:    

    (gdb) rbreak test.c:.
    Breakpoint 1 at 0x40058f: file test.cpp, line 12.
    void func1(void);


#### Enable logging
    (gdb) set pagination off
    (gdb) set print pretty
    (gdb) set logging file ./test.log
    (gdb) set logging on
    Copying output to ./test.log.

#### Tell gdb to print backtrace and continue without asking when it hits  a breakpoint , 1-8 below is total breakpoint    
    (gdb) commands 1-8
    Type commands for breakpoint(s) 1-10, one per line.
    End with a line saying just "end".
    >bt
    >c
    >end


#### Start  the program    
    (gdb) r
    Starting program: a.out


#### Once the program finishes, it would have dumped the logs to the disk in test.log    

    $ cat test.log
    
    Breakpoint 9, main () at test.cpp:15
    15              func1();
    #0  main () at he.c:15

    Breakpoint 1, func1 () at test.cpp:12
    12      void func1(void) { func2(); }
    #0  func1 () at he.c:12
    #1  0x00000000004005a0 in main () at he.c:15

## Part II: run gen_graph.py on collected data

### Dependencies    

    sudo apt-get install graphviz
    sudo python3 -m pip install -r requiments.txt
    cat requiments.txt 
        matplotlib
        pydotplus


### Now run gen_graph.py    

    $ python gen_graphs.py
    Usage:
    python3 ./gen_graphs.py  <gdb_backtrace_logs>
    

    $ python3 gen_graphs.py test.log
    [1] parsing gdb logs..
    [2] adding nodes..
            # of nodes: 10
    [3] adding edges..
            # of edges: 9
    [4] saving graph to:
            /home/vagrant/gdb_graphs/test.svg

Open the svg file in a browser




