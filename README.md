# ComplexCi

This project (ComplexCi) mainly focuses on the c++ implementation of Collective Influence (CI) algorithm, which is designed to find the most important node (or influencers) in the Complex Network via optimal percolation developed by:

> Morone F, Makse H A. Influence maximization in complex networks through optimal percolation[J]. Nature, 2015, 524(7563): 65-U122. 

> Morone F, Min B, Bo L, et al. Collective Influence Algorithm to find influencers via optimal percolation in massively large social media[J]. Scientific reports, 2016, 6.  

  

## Algorithm  
  
Overall, the target of CI algorithm is to give a ranking list of nodes according to their importance and the top-ranked nodes will have more importance. We can remove the nodes from the top-ranked ones in the ranking list generated by CI algorithm and calculate the size of giant component after each removal, which will break down the network into many disconnected pieces. The ratio of giant component will reach zero with the one-by-one removal operation finally. Therefore,  the better algorithm,  the sooner the network will collapse to the zero giant component with smaller count of provided nodes.

Users can get the minimal set of influencers of the Complex Network by the C++ program in this repository . Considered that CI algorithms are able to be used in so many scientific fields , I implement this algorithm in modern C++ style code. Compared with the original c code http://www-levich.engr.ccny.cuny.edu/~hernanlab/uploads/CI_HEAP.c mentioned on the http://www-levich.engr.ccny.cuny.edu/webpage/hmakse/software-and-data/ with above paper , the ComplexCi has the following features :

* This project is using the c++ code style of modern oriented object programming with Standard Template Library (STL), which is easier for users to modify than the original c code. After reviewing the original c language code CI_HEAP.c, I believe it is difficult for users to read , maintain and implement it if they have their own idea, especially CI_HEAP.c contains lots of simple variable abbreviation, multilevel pointer, unsafe memory management and unfriendly c style code.
* ComplexCi accepts more input parameters and they can be used in more flexible  behavior of operating nodes in the provided network. 
  * Users can determine the batch size of deleting nodes in ComplexCi per updating CI values when the Complex Network collapses
  * Users can determine the certain giant component ratio when starting re-inserting algorithm which complex network collapses to
  * Users can determine whether they need re-insert operation in the CI algorithm to optimize the result
* Verified that the result of traditional c implementation and new c++ can both achieve the nearly same result in the metric of Robustness, even the new c++ implementation is more efficient and spends much less time on some datasets than the traditional c program. See in the Benchmark Section
* The traditional CI_HEAP.c is merged into this project as well. Users can switch the option to use the new c++ designer or the traditional c style code depending on their taste.

## Usage

This section describes the Usage of the ComplexCi and its corresponding scripts

###  Get Repository

####  Download Release

####  Fetch from Github Source and Compile

Users can directly clone the repository by the git command or just download the zip archiver on the webpage

```bash
git clone https://github.com/zhfkt/ComplexCi.git
```
There are lots of C++11 features and syntax in the code so that the C++ Compiler needs to support C++11 Standard . In fact, there is only one cpp file ComplexCi/ComplexCi.cpp need to be compiled and it doesn’t rely on the other extra library

* Linux

Users can enter into the “bin/” under root project folder and execute 

```bash
g++ -pthread  ../ComplexCi/ComplexCi.cpp  -o ComplexCi -O3 -std=c++11 
```

or just execute

```bash
./make.sh
```

to generate binary bin/ComplexCi . Pls notice that the version of g++ needs to support c++11. For my own dev, the compilation is passed under g++ 5.4.0 on Ubuntu 16.04. Otherwise , you will get failure of several incorrect syntax errors.

* Windows

There has already been a windows binary file x64/Release/ComplexCi.exe compiled on the Windows7 x64 in the repository. It can just be used if users do not need to recompile or change the code. If users want to compile themselves and have the Visual Studio 2013 or higher version, they can directly open the ComplexCi.sln and compile the code in the IDE. The binary file will be generated under x64/Release/ComplexCi.exe .  I believe users can also pick up any other IDE or Compiler supporting C++11 to compile the file ComplexCi/ComplexCi.cpp . Pls note that replace the x64/Release/ComplexCi.exe with your own generate file after compilation.


###  Run

####  Network Input File Format

The data of network can be written in one txt/csv files and the network is considered as undirected network. Each row contains 2 node IDs divided by one comma, which means there is a connection between these 2 nodes. The node IDs should be integer and started from 0. For example:

```
0,1
1,2
1,0
2,1
....
```

The complete Example is at data/test/karate.csv

####  Easy Start to use Script

Using Script in the project is a quick start to utilize Collective Influence (CI) algorithm. There are 3 scripts users can execute in the project.

* __traditionalCollectiveInfluence__

Users can use this script to utilize traditional c style code of http://www-levich.engr.ccny.cuny.edu/~hernanlab/uploads/CI_HEAP.c . I merge it into this repository and set the compatible interface layer to call the c code in c++ .

Script “traditionalCollectiveInfluence” accepts 3 parameters:

```bash
./traditionalCollectiveInfluence.sh <networkPath> <ballRadius> <isPrintMinPointCausingMinComponent>
```


<networkPath> is the file path. The file format is described in the section Network Input File Format
<ballRadius> is the Radius parameter defined in the Collective Influence Algorithm
When the ballRadius is zero, pls notice that Collective Influence Algorithm will degenerate into HDA (high degree adaptive) algorithm. a.k.a. CI value of each node will be equal with degree of the node.
<isPrintMinPointCausingMinComponent> whether the output file contains limited point leading to 0.01 of giant component ratio or all points. If it is set to 0, the program will output all nodes. Otherwise the program will output partial points, which will make the giant component ratio to 0.01 in the deleting nodes process




