# IHPCSS Challenge #

You are taking part to the [International High-Performance Computing Summer School](https://ss21.ihpcss.org) programming challenge? That's where it starts!

## Table of contents ##

* [What is the challenge?](#what-is-the-challenge)
* [What is this repository for?](#what-is-this-repository-for)
* [How do I get set up?](#how-do-i-get-set-up)
  * [Download the source codes](#download-the-source-codes)
  * [Compile the source codes](#compile-the-source-codes)
  * [Submit to Bridges compute nodes](#submit-to-bridges-compute-nodes)
  * [Verification](#verification)
* [What kind of optimisations are not allowed?](#what-kind-of-optimisations-are-not-allowed)
* [Send your solution to the competition](#send-your-solution-to-the-competition)
* [Who do I talk to?](#who-do-i-talk-to)
* [Acknowledgments](#acknowledgments)

## What is the challenge? ##

This challenge introduces a simple problem: take a metal plate, put a flame below it and simulate the temperature propagation across that metal plate. For simplicity, we assume that the area touched by the flame will always be at the same temperature (arbitrarily put at 50 degrees; yes it is a tiny flame). The rest of the metal plate however will start heating up as the heat propagates outwards the lighter zone. 

The challenge is that you have 30 seconds to process as many iterations as possible. Of course, this is equivalent to asking you to decrease the runtime; the faster your iterations, the more you can execute under 30 seconds.

[Go back to table of contents](#table-of-contents)
## What is this repository for? ##

* You will find here everything you need to compete; source codes, makefiles, documentation, scripts, tests...
* You can for both tracks:
  * Classic parallel programming: MPI + OpenMP
  * Accelerator: MPI + OpenACC
* Each of these is available
* It provides you with a pre-setup experimental protocol; it makes sure contestants compete in the same conditions and allows to compare experiments fairly.

[Go back to table of contents](#table-of-contents)
## How do I get set up? ##
### Download the source codes ###
All you have to do is clone this repository: ```git clone https://github.com/capellil/IHPCSS_Programming_challenge_2021.git```.

Note that you are strongly encouraged to work on the source files provided instead of making copies. To keep it short, you will discover in the sections below that multiple scripts have been written to make your life easier (makefile, submitting to compute nodes etc..). However, these scripts are designed to work with the files provided, not the arbitrary copies you could make.

[Go back to table of contents](#table-of-contents)
### Compile the source codes ###
There is a makefile as you can see; it will compile all versions and generate the corresponding binaries in a folder ```bin```. OpenACC requires the PGI compiler, so we use the PGI compiler over all versions to keep things consistent. Make sure you load the right module with ```module load cuda/9.2 mpi/pgi_openmpi/19.4-nongpu``` before making, if you do not, the makefile will remind you.

What happens behind the scene?

As you will quickly see, there is one folder for C source codes, one for FORTRAN source codes. Inside, each version has a specific file:

| Model | C version | FORTRAN version |
|-------|-----------|-----------------|
| MPI + OpenMP | cpu.c | cpu.F90 |
| MPI + OpenACC | gpu.c | gpu.F90 |

And of course, modify the file corresponding to the combination you want to work on. No need to make a copy, work on the original file, everything is version controlled remember.

[Go back to table of contents](#table-of-contents)
### Submit to Bridges compute nodes ###
(***Note**: Jobs submitted with this script use the reservation queue ```challenge```, which becomes active on Monday the 8th of July 2019 at 8:00pm local. No need to submit your jobs before that time then because they will be queued but will not be executed until the reservation queue becomes active.*)

Similarly to the section "Run locally", a script has been written for you to easily submit your work to Bridges via SLURM: ```./submit.sh LANGUAGE IMPLEMENTATION SIZE OUTPUT_FILE```. The parameters LANGUAGE, IMPLEMENTATION and SIZE are identical to that passed to the ```run.sh``` script. The output file this time is no longer optional however, because you need a file to which redirect the output of your job.

How does it work? As you have probably seen, there is a ```slurm_scripts``` folder. It contains two SLURM submission scripts for each version (serial, OpenMP, MPI etc...): one for the small grid, one for the big grid. That allows each SLURM script to be tailored (number of nodes, type of nodes, walltime...) for the implementation and size demanded.

If you want to create your own SLURM submission script, there is an additional one called ```general.slurm```, which contains all the commands you may want to use along with their description. You will also find in that submission script links that will lead you to PSC webpages containing more information about submission scripts.

[Go back to table of contents](#table-of-contents)
### Verification ###
For the small

[Go back to table of contents](#table-of-contents)
## What kind of optimisations are not allowed? ##

* Changing the compilation process (that is: using different compilers, compiler flags, external libraries etc...). The point in this challenge is not for you to read hundreds of pages of documentation to find an extra flag people may have missed.
* Changing the running process; provided scripts already use a sensible configuration. Again, you only have a few days; the objective of the hybrid challenge is for you to play with the code, not spend hours defining the best MPI / OpenMP ratio for instance.
* Changing the submission process; such as using more than 4 nodes for instance.
* Changing the algorithm; yes it is a naive one but it exposes good characteristics for you to practice what you have learned in OpenMP, MPI and OpenACC.
* Reducing the amount of work to be done such as ignoring the cells whose value will be zero during the entire simulation.
* Removing the track_progress from the loop or changing the frequency at which it prints.
* Bypassing the buffer copy using a pointer swap.
* Decreasing the accuracy of the calculations by switching from doubles to floats.
* You are not sure about whether a certain optimisation is allowed or not? Just ask :)

[Go back to table of contents](#table-of-contents)
## Send your solution to the competition ##
Participating to the hybrid challenge is for fun; to practice what you have learned during the IHPCSS. But if you want to see how far you got, you can send your solution for it to be assessed and evaluated as part of the competition, and know if you managed to develop the fastest code of your category (CPU or GPU). By the way, the team who will have developed the fastest CPU solution will win the trophy shown at the last slide in the IHPCSS_Coding_challenge_intro.pdf file, and identically for the fastest GPU solution of course.

If you want your solution to be assessed, send an email **by Wednesday 28th of July 2021 11:59PM AOE** to CAPELLI Ludovic (email address in the ```IHPCSS_Coding_challenge_intro.pdf``` file; next to last slide) containing:
* The full name of each team member (no more than 3 members per team remember), because if your team wins we need to know who we should call on stage :)
* The source file of the version you optimised. Typically, it will certainly mean:
  * ```hybrid_cpu.c``` (or ```hybrid_cpu.F90```) if you focused on CPU using the MPI + OpenMP version. Possibly ```mpi.c``` (or mpi.F90) if you focused on CPU using the MPI only version instead.
  * ```hybrid_gpu.c``` (or ```hybrid_gpu.F90```) if you focused on GPU using the MPI + OpenACC version.

**Note**: there is no need to send the ```makefile``` / ```submit.sh``` scripts or else, send just the source file of the version you optimised. Your code will be compiled and run using the original ```makefile``` / ```submit.sh``` scripts provided, on the ```big``` grid. This way, every participant has their code compiled & run in strictly identical conditions.

[Go back to table of contents](#table-of-contents)
## Who do I talk to? ##

* CAPELLI Ludovic

(My email addresses are in the ```IHPCSS_Coding_challenge_intro.pdf``` file; next to last slide. You can also find me on slack.)

[Go back to table of contents](#table-of-contents)
## Acknowledgments ##
* [John Urbanic](https://www.psc.edu/staff/urbanic)
* [David Henty](https://www.epcc.ed.ac.uk/about/staff/dr-david-henty)

[Go back to table of contents](#table-of-contents)
