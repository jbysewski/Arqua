# Arqua - Architecture Quality Analyzer
Arqua is a tool used to visualize software structure and complexity.

Arqua parses RTL, an intermediate format used by gcc, and generates a graph in 
the dot language to be parsed by [Graphviz](http://www.graphviz.org). The graph 
visualizes complex functions, files and directories as well as tangles between 
these.

## Installing Arqua
Install Arqua using make:

	make install

This will copy the script and the man page to the directory structure specified 
by the _prefix_ variable (default is /usr/local).

## Using Arqua
Use the following command to get gcc to write an RTL file when compiling:

	gcc -c -fdump-rtl-expand file.c

This generates an RTL file, usually with the name _file.c.104r.expand_ (the 
name may vary from system to system). Now use Arqua and generate a dot file:

	arqua -functions file.c.104r.expand > file.dot

The file can now be viewed directly with a dot file viewer or you can use the 
Graphviz tools to generate an image, for instance a png image:

	dot file.dot -Tpng -ofile.png

Run Arqua on a directory structure of files by inputing all files with their 
relative path to Arqua:

	arqua -files -packages dir1/file1.c.104.r.expand dir2/file2.c.104.r.expand > dirs.dot

This will generate a graph visualizing both files and directories.

## Arqua options
There are a few options that controls how Arqua is creating a report:

* -report <graph|text> (default:graph) Arqua can create text list of smells 
instead of a graph using -report text. When used all other options are ignored.
* -root [path] (default:<working directory>) Sets what root directory to start 
from when generating a graph. The analysis and calculation is still done from 
the working directory.
* -packages (default:off) Tells arqua to draw packages (directories).
* -files (default:off) Tells arqua to draw files.
* -functions (default:off) Tells arqua to draw functions.
* -levels [number of levels] (default:all) Sets the number of abstraction levels 
to traverse.
* -ft [function complexity threshold] (default:10) Sets the function complexity 
threshold (max allowed cyclomatic complexity of the RTL representation of the 
function))
* -nt [node complexity threshold] (default:35) Set the node (directory and file) 
complexity threshold (max number of allowed edges in a node)

## Examples
In each example it is assumed that _$FILES_ contains a list of RTL files with 
their path relative to the root of the system.

### Example 1
Draw the top 3 directories and all files directly within them:

	arqua -packages -files -levels 3 $FILES > system.dot
	dot system.dot -Tpng -osystem.png

### Example 2
Generate a list of smells of a system:

	arqua -report text $FILES > system.txt

### Example 3
Draw the files and their internal call graphs for all files within the folder 
adir/bdir:

	arqua -files -functions -levels 3 -root adir/bdir $FILES > system.dot
	dot system.dot -Tpng -osystem.png

Note that -root only filter out data and that -levels is still needed to 
traverse down into the directories.

## Contributions
Any code contributions or ideas on how to even better represent the structure of 
C code are welcome.

## License
Copyright (c) 2012, Magnus Ernstsson
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met: 

1. Redistributions of source code must retain the above copyright notice, this
   list of conditions and the following disclaimer. 
2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution. 

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

