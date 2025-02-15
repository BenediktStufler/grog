grogue -- Generate Random Blockstable Graphs

* 
* 1. Introduction
* 

This project implements efficient samplers for random connected graphs from selected subcritical block-stable classes. It is based on an R-enriched tree encoding of such graphs and a resulting coupling with simply generated trees. This way, graphs may be viewed as blow-ups of trees, where for each vertex v with children v1, ..., v_k we remove the edges between v and its children and add new edges with endpoints in {v, v_1, ..., v_k} according a random choice that only depends on the outdegree k. These edges are called the decoration of v.

The algorithm proceeds in two steps. First, it generates the underlying simply generated tree using a multithreaded version of an algorithm by Devroye (2012, SIAM Journal of Computing). Second, it generates the decorations of vertices in two phases. In the first phase, the decoration of vertices with small outdegree are generated using precomputed probability weights. In the second phase, decorations of the remaining vertices with large outdegree are generated using a multi-threaded coupon collection algorithm. Each thread uses a Boltzmann sampling procedure to generate randomly sized decorations which are added to the coupon list if the size matches the outdegree of one of the vertices missing a decoration. The parameter for the Boltzmann sampler is adjusted so that the roughly expected size matches the range of the outdegrees.

The program may be instructed to output lists of vertex parameters (degree profile, height profile, closeness centrality) of the generated graph, ordered according to a breadth-first-search of the underlying tree.

Currently, the following graph classes are supported:
	- Trees
	- Cactus Graphs 
	- Outerplanar Graphs

Support for series-parallel graphs will be added later on.


*
* 2. Usage
*
grogue -- Generate Random blOck-stable Graphs

Usage: grogue [-?V] [-c CENTFILE] [-d DEGFILE] [-D MAXDEGFILE] [-g GRAPHCLASS]
            [-h HEIGHTFILE] [-H MAXHEIGHTFILE] [-i INPUTFILE] [-N NUM]
            [-o OUTFILE] [-p PROFILE] [-r RANDGEN] [-s SIZE] [-S SEED]
            [-t THREADS] [-v VERTEX] [--centfile=CENTFILE] [--degfile=DEGFILE]
            [--maxdegfile=MAXDEGFILE] [--graphclass=GRAPHCLASS]
            [--heightfile=HEIGHTFILE] [--maxheightfile=MAXHEIGHTFILE]
            [--inputfile=INPUTFILE] [--num=NUM] [--outfile=OUTFILE]
            [--profile=PROFILE] [--randgen=RANDGEN] [--size=SIZE] [--seed=SEED]
            [--threads=THREADS] [--vertex=VERTEX] [--help] [--usage]
            [--version] 

  -c, --centfile=CENTFILE    Output a list of the vertices' closeness
                             centrality to CENTFILE.
  -d, --degfile=DEGFILE      Output a list of the vertices' degrees to DEGFILE.
                            
  -D, --maxdegfile=MAXDEGFILE   Output maximal vertex degree to  MAXDEGFILE.
  -g, --graphclass=GRAPHCLASS   Sample graphs from GRAPHCLASS. Currently
                             supported: 'tree' for the class of trees, 'cacti'
                             for the class of cactus graphs, 'outer' for the
                             class of outerplanar graphs
  -h, --heightfile=HEIGHTFILE   Output the height profile to HEIGHTFILE.
  -H, --maxheightfile=MAXHEIGHTFILE
                             Output maximal vertex height to MAXHEIGHTFILE.
  -i, --inputfile=INPUTFILE  Read a _connected_ graph from file INFILE (graphml
                             format) instead of generating it at random.
  -N, --num=NUM              Simulate NUM many samples. Requires the use of the
                             % symbol in all specified output filenames. For
                             example, --num=100 --outfile=graph%.graphml will
                             create the files graph001.graphml,
                             graph002.graphml, ..., graph100.graphml. If the
                             filename does not contain a % symbol then the file
                             will be opend in append mode.
  -o, --outfile=OUTFILE      Output simulated graph in graphml format to
                             OUTFILE.
  -p, --profile=PROFILE      Output the degree profile to the file PROFILE.
  -r, --randgen=RANDGEN      Use the pseudo random generator RANDGEN. Available
                             options are taus2, gfsr4, mt19937, ranlux,
                             ranlxs0, ranlxs1, ranlxs2, ranlxd1, ranlxd2, mrg,
                             cmrg, ranlux389. The default is taus2.
  -s, --size=SIZE            Generate a connected graph with SIZE vertices.
  -S, --seed=SEED            Specify the seed of the random generator in the
                             first thread. Thread number k will receive SEED +
                             k - 1 as seed. The default is to set SEED to the
                             systems timestamp (in seconds).
  -t, --threads=THREADS      Distribute the workload on THREADS many threads.
                             The default value is the number of CPU cores.
  -v, --vertex=VERTEX        Specify a root vertex. Used in conjunction with
                             the --inputfile parameter. 
  -?, --help                 Give this help list
      --usage                Give a short usage message
  -V, --version              Print program version

Mandatory or optional arguments to long options are also mandatory or optional
for any corresponding short options.

Report bugs to <benedikt.stufler@posteo.net>.



*
* 3. Example
*

Here are some examples for the Linux command line:

Simulate a random outerplanar graph with 10 vertices and write the output to the terminal:

grogue -g outer -s 10 -o /dev/stdout


Simulate ten cactus graphs with one million vertices each and write them and their degree sequences to disk:

grogue -g cacti -s 1000000  -N 10 -o graph%.graphml -d deglist%.txt


Simulate 1000 cactus graphs with 1000 vertices each and form the average of their heights. Requires the datamash package.
./grogue -g cacti -s 1000 -N 1000 -H /dev/stdout | datamash mean 1



*
* 4. License
*

Copyright (C) 2021 Benedikt Stufler

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

