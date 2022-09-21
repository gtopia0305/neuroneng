# Introduction to How to Use Conda

Anaconda is a distribution that provides a collection of packages written in Python and R programming languages in the scientific computing field (data science, machine learning application programs, large-scale data processing, predictive analytics, etc.).

The Anaconda distribution has over 12 million users, and it comprises more than 1400 popular data science packages suitable for Windows, Linux, and MacOS.

To install Anaconda, you can download and install the distribution suitable for your OS from the [https://www.anaconda.com](https://www.anaconda.com/) website.

(Example) Windows, MacOS, Linux



Currently, Anaconda provides versions based on Python 3.7 and Python 2.7, respectively.



Conda is an application provided to manage package versions in Anaconda.

By using conda, the dependency problem that Python users have the most difficulty with when installing packages can be easily addressed.



This document introduces how to use the conda package in the KISTI system for Python users.

The "/home01/userID" directory on the introduction page is the home directory of the test account. Therefore, users are required to change it to the path that is appropriate for them.

## 1. Use of Conda

For Miniconda, you can download a version suitable for each OS from the https://docs.conda.io/en/latest/miniconda.html site.

For Anaconda, you can download a version suitable for each OS from the [https://www.anaconda.com/distribution/#download-section](https://www.anaconda.com/distribution/#download-section) site.

| **Command** | **Description**                                                                                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| clean       | Discard unused packages and caches.                                                                                                                                       |
| config      | <p>Modify configuration values in .condarc. This is modeled after the git config command.</p><p>Write to the user .condarc file (/home01/userID/.condarc) by default.</p> |
| create      | Create a new conda environment from a list of specified packages.                                                                                                         |
| help        | Display a list of available conda commands and their help strings.                                                                                                        |
| info        | Display information on current conda installation.                                                                                                                        |
| init        | Initialize conda for shell interaction. \[Experimental]                                                                                                                   |
| install     | Install a list of packages into a specified conda environment.                                                                                                            |
| list        | List linked packages in a conda environment.                                                                                                                              |
| package     | Low-level conda package utility. (EXPERIMENTAL)                                                                                                                           |
| remove      | Discard a list of packages from a specified conda environment.                                                                                                            |
| uninstall   | Alias for conda removal.                                                                                                                                                  |
| run         | Run an executable in a conda environment. \[Experimental]                                                                                                                 |
| search      | <p>Search for packages and display associated information.<br>The input is a MatchSpec, a query language for conda packages.</p><p>Refer to examples below.</p>           |
| update      | Updates conda packages to the latest compatible version.                                                                                                                  |
| upgrade     | Alias for conda update.                                                                                                                                                   |

## 2. Create Conda Environment

\- The conda environment creates an independent virtual execution environment for Python; hence, it is easy to manage versions of packages.

\- You can create a conda environment using "conda create -n \[ENVIRONMENT].”

\- The conda environment is created with the environment name specified in the path under envs of the conda path by default.

\- If the "--use-local" option is adopted, the conda environment is created in the user’s home directory (**${HOME}/.conda/envs/\[environment\_name]**).



\- Example -

```
$ module load python/3.7.1
$ conda create -n scikit-learn_0.21 --use-local
Collecting package metadata: done
Solving environment: done
 
## Package Plan ##
 
environment location: /home01/userID/.conda/envs/scikit-learn_0.21
 
Proceed ([y]/n)? y
 
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use:
# > conda activate scikit-learn_0.21
#
# To deactivate an active environment, use:
# > conda deactivate
#
 
$ source activate scikit-learn_0.21
(scikit-learn_0.21) $
```

## 3. Install and Check Packages in Conda Environment

\- Packages can be installed using conda install \[package name].

\- Packages in the conda channel can be installed using "conda install -c \[channel name] \[package name].”

\- Packages are installed under the conda environment path created in Section 2.



\- Example -

```
$ module load python/3.7.1
$ source activate scikit-learn_0.21
(scikit-learn_0.21) $ conda install scikit-learn
Collecting package metadata: done
Solving environment: done

## Package Plan ##
 
  environment location: /home01/userID/.conda/envs/scikit-learn_0.21
  added / updated specs:
    - scikit-learn
 
The following packages will be downloaded:
    package                    |            build
    ---------------------------|-----------------
    ca-certificates-2019.1.23  |                0         126 KB
    ...
      wheel-0.33.1               |           py37_0          39 KB
    ------------------------------------------------------------
                                           Total:       277.6 MB
 
The following NEW packages will be INSTALLED:

  blas               pkgs/main/linux-64::blas-1.0-mkl
 ...
   zlib               pkgs/main/linux-64::zlib-1.2.11-h7b6447c_3
 
Proceed ([y]/n)? y
 
Downloading and Extracting Packages
setuptools-40.8.0    | 643 KB    | ##################################### | 100% 
...
openssl-1.1.1b       | 4.0 MB    | ##################################### | 100% 
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
(scikit-learn_0.21) $ python -c "import sklearn"
(scikit-learn_0.21) $
```

## 4. Check Conda Environment List

\- You can check the conda environment list using "conda-env list" or "conda env list.”



\-Example-

```
(scikit-learn_0.21) $ conda env list
# conda environments:
#
base                     /apps/applications/PYTHON/3.7.1
scikit-learn_0.21     *  /home01/userID/.conda/envs/scikit-learn_0.21
(scikit-learn_0.21) $ conda deactivate
$
```

## 5. Remove Conda Environment

\- You can remove a conda environment using "conda-env remove -n \[ENVIRONMENT]" or "conda env remove -n \[ENVIRONMENT].”



\-Example-

```
$ module load python/3.7.1
$ conda env remove -n scikit-learn_0.21
Remove all packages in environment /home01/userID/.conda/envs/scikit-learn_0.21:
$
```



## 6. Export Conda Environment

\- The conda-pack package is required before exporting a conda environment.

(Reference) [https://conda.github.io/conda-pack](https://conda.github.io/conda-pack)

\- You can adopt the conda environment in another system by using "conda pack -n \[ENVIRONMENT] -o \[file name].”

(Example) When the external Internet is not connected, the same conda environment is used in another system

\-Example-

```
$ module load python/3.7.1
$ source activate tensorflow_1.12
(tensorflow_1.12) $ conda install -c conda-forge -n tensorflow_1.12
(tensorflow_1.12) $ conda pack -n tensorflow_1.12 -o conda_tensorflow_1.12.tar.gz
Collecting packages...
Packing environment at '/home01/userID/.conda/envs/tensorflow_1.12' to 'conda_tensorflow_1.12.tar.gz'
[########################################] | 100% Completed | 4min 18.8s
(tensorflow_1.12) $ ls -l conda_tensorflow_1.12.tar.gz
-rw-------. 1 userID in0162 1459826406 Mar 28 15:03 conda_tensorflow_1.12.tar.gz
(tensorflow_1.12) $
```

## 7. Import Conda Environment

\- You can import the conda environment that was created using conda pack, as presented in the following example, and use it after setting up the environment.



\-Example-

```
$ module load python/3.7.1
$ mkdir -p $HOME/.conda/envs/tensorflow_1.12
$ tar xvzf conda_tensorflow_1.12.tar.gz -C $HOME/.conda/envs/tensorflow_1.12
$ source activate tensorflow_1.12
(tensorflow_1.12) $ conda deactivate
$
```

{% hint style="info" %}
2021년 12월 2일에 마지막으로 업데이트되었습니다.
{% endhint %}
