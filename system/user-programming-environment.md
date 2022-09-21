# User Programming Environment

## A. Programming Tool Installation Status

![](../.gitbook/assets/programming\_tool\_installation\_status.png)

※ We recommend using the Anaconda environment for the artificial intelligence framework of the Neuron system. We also plan to provide an environment that uses Singularity to run container images based on user requirements.

※ Using Singularity: Refer to “\[Appendix 3] Method for Using Singularity Container Images”

<mark style="color:red;">**※ Non-CUDA MPI libraries must be employed to use nodes that are not equipped with GPUs.**</mark>

\


## B. How to Use the Compilers

#### **1) Compiler and MPI environment configuration (modules)**

#### **(1) Module-related basic commands**

ㅇ Print a list of available modules

You can check a list of available modules, such as compilers and libraries.

```
$ module avail
or
$ module av
```

ㅇ Add a module to be used

You can add modules that you plan to use, such as compilers and libraries.

Modules that will be used can be added simultaneously.

```
$ module load [module name] [module name] [module name] ...
or
$ module add [module name] [module name] [module name] ...
```

ㅇ Delete used modules

You can remove unnecessary modules. Several modules can be deleted simultaneously.

```
$ module unload [module name] [module name] [module name] ...
or
$ module rm [module name] [module name] [module name] ...
```

ㅇ Print a list of used modules

You can check the list of modules that are currently configured.

```
$ module list
or
$ module li
```

ㅇ Delete all used modules simultaneously.

```
$ module purge
```

ㅇ Check the module installation path.

```
$ module show [module name]
```

#### 2) Compiling sequential programs

A sequential program is a program that does not consider the parallel program environment. That is, it is a program that does not use a parallel program interface, such as OpenMP or MPI. A sequential program is run using only one processor in one node. Compiler-specific options used for compiling sequential programs are also used when compiling parallel programs. Hence, reference should be made to these options even if you are not interested in sequential programs.

#### ① Intel compiler

To use an Intel compiler, add the required version of the Intel compiler module. Available modules can be checked using the “module avail” command.

```
$ module load intel/18.0.2
```

※ Check available versions by referring to the programming tool installation status table.

#### ■ Compiler types

| Compiler   | Program | Source file extension                                  |
| ---------- | ------- | ------------------------------------------------------ |
| icc / icpc | C / C++ | .C, .cc, .cpp, .cxx,.c++                               |
| ifort      | F77/F90 | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

#### ■ Intel compiler usage example

The following is an example of compiling a test sample file using an Intel compiler to generate a test.exe executable file.

```
$ module load intel/18.0.2
$ icc -o test.exe test.c
or
$ ifort -o test.exe test.f90
$ ./test.exe
```

※ You can copy a test sample file for job submission from /apps/shell/job\_examples and use it.

**② GNU compiler**

To use a GNU compiler, add the required version of the GNU compiler module. Available modules can be checked using the “module avail” command.

```
$ module load gcc/8.3.0
```

※ Check available versions by referring to the programming tool installation status table.

※ You must use version "gcc/4.8.5" or higher.

#### ■ Compiler types

| Compiler  | Program | Source file extension                                  |
| --------- | ------- | ------------------------------------------------------ |
| gcc / g++ | C / C++ | .C, .cc, .cpp, .cxx,.c++                               |
| gfortran  | F77/F90 | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

#### ■ GNU compiler usage example

The following is an example of compiling a test sample file using a GNU compiler to generate a test.exe executable file.

```
$ module load gcc/8.3.0
$ gcc -o test.exe test.c
or
$ gfortran -o test.exe test.f90
$ ./test.exe
```

※ You can copy a test sample file for job submission from /apps/shell/job\_examples and use it.

#### ③ PGI compiler

To use a PGI compiler, add the required version of the PGI compiler module. Available modules can be checked using the “module avail” command.

```
$ module load pgi/19.1
```

※ Check available versions by referring to the programming tool installation status table.

**■ Compiler types**

| Compiler     | Program | Source file extension                                  |
| ------------ | ------- | ------------------------------------------------------ |
| pgcc / pgc++ | C / C++ | .C, .cc, .cpp, .cxx,.c++                               |
| pgfortran    | F77/F90 | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

**■ PGI compiler usage example**

The following is an example of compiling a test sample file using a PGI compiler to generate a test.exe executable file.

```
$ module load pgi/19.1
$ pgcc -o test.exe test.c
or
$ pgfortran -o test.exe test.f90
$ ./test.exe
```

※ You can copy a test sample file for job submission from /apps/shell/job\_examples and use it.

#### 3) Compiling parallel programs

#### (1) OpenMP compiling

OpenMP is a technique that has been developed to utilize multi-threading by only using compiler directives. The compiler used to compile a parallel program using OpenMP is the same as the compiler used for a sequential program. Parallel compiling can be performed by adding compiler options, and most compilers currently support the OpenMP directives.

| Compiler option          | Program           | Option   |
| ------------------------ | ----------------- | -------- |
| icc / icpc / ifort       | C / C++ / F77/F90 | -qopenmp |
| gcc / g++ / gfortran     | C / C++ / F77/F90 | -fopenmp |
| pgcc / pgc++ / pgfortran | C / C++ / F77/F90 | -mp      |

#### ① Example of compiling an OpenMP program (Intel compiler)

The following is an example of compiling a test\_omp sample file using openMP with an Intel compiler to generate a test\_omp.exe executable file.

```
$ module load intel/18.0.2
$ icc -o test_omp.exe -qopenmp test_omp.c
or
$ ifort -o test_omp.exe -qopenmp test_omp.f90
$ ./test_omp.exe
```

#### ② Example of compiling an OpenMP program (GNU compiler)

The following is an example of compiling a test\_omp sample file using openMP with a GNU compiler to generate a test\_omp.exe executable file.

```
$ module load gcc/8.3.0
$ gcc -o test_omp.exe -fopenmp test_omp.c
or
$ gfortran -o test_omp.exe -fopenmp test_omp.f90
$ ./test_omp.exe
```

#### ③ Example of compiling an OpenMP program (PGI compiler)

The following is an example of compiling a test\_omp sample file using openMP with a PGI compiler to generate a test\_omp.exe executable file.

```
$ module load pgi/19.1
$ pgcc -o test_omp.exe -mp test_omp.c
or
$ pgfortran -o test_omp.exe -mp test_omp.f90
$ ./test_omp.exe
```

#### (2) MPI compiling

The user can run the MPI commands in the following table. These commands are a sort of wrapper, and the compiler that is specified by the .bashrc file compiles the source file.

| **Category**  | **Intel** | **GNU**  | **PGI**   |
| ------------- | --------- | -------- | --------- |
| Fortran       | ifort     | gfortran | pgfortran |
| Fortran + MPI | mpiifort  | mpif90   | mpif90    |
| C             | icc       | gcc      | pgcc      |
| C + MPI       | mpiicc    | mpicc    | mpicc     |
| C++           | icpc      | g++      | pgc++     |
| C++ + MPI     | mpiicpc   | mpicxx   | mpicxx    |

Even if compiling is performed using mpicc, it is necessary to use the options that correspond to the original compiler being wrapped.

#### ① Example of compiling an MPI program (Intel compiler)

The following is an example of compiling a test\_mpi sample file using MPI with an Intel compiler to generate a test\_mpi.exe executable file.

```
$ module load intel/18.0.2 impi/18.0.2
$ mpiicc -o test_mpi.exe test_mpi.c
or
$ mpiifort -o test_mpi.exe test_mpi.f90
$  srun ./test_mpi.exe
```

#### ② Example of compiling an MPI program (GNU compiler)

The following is an example of compiling a test\_mpi sample file using MPI with a GNU compiler to generate a test\_mpi.exe executable file.

```
$ module load gcc/8.3.0 openmpi/3.1.5
$ mpicc -o test_mpi.exe test_mpi.c
$ or 
$ mpif90 -o test_mpi.exe test_mpi.f90
$ srun ./test_mpi.exe
```

#### ③ Example of compiling an MPI program (PGI compiler)

The following is an example of compiling a test\_mpi sample file using MPI with a PGI compiler to generate a test\_mpi.exe executable file.

```
$ module load pgi/19.1 openmpi/3.1.5
$ mpicc -o test_mpi.exe test_mpi.c
or
$ mpifort -o test_mpi.exe test_mpi.f90
$ srun ./test_mpi.exe
```

{% hint style="info" %}
2021년 12월 1일에 마지막으로 업데이트되었습니다.
{% endhint %}
