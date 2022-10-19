# How to Use Conda

Anaconda is a distribution that provides a collection of packages written in Python and R programming languages in the scientific computing field (data science, machine learning application programs, large-scale data processing, predictive analytics, etc.).

&#x20;

※ For detailed information on how to use conda, refer to the KISTI Homepage > Technical Support > Guidelines > Neuron > Software > Introduction to How to Use Conda.

&#x20;  ****  &#x20;

◦ Use conda and create a conda environment

```
$ module load python/3.7.1
$ conda create -n [conda_env_name] --use-local
$ conda env list
```

※ \[conda\_env\_name]: the environment name to be used

※ Conda environment creates an independent virtual execution environment for Python; hence, it is easy to manage versions of packages.

※ If the "--use-local" option is adopted, the conda environment is created in the user’s home directory (**${HOME}/.conda/envs/\[environment\_name]**).

※ Load all modules to be used before creating the conda environment.



◦ Activate the conda environment

```
$ source activate [conda_env_name]
 ([conda_env_name])l
```



◦ Install and check packages in the conda environment

```
([conda_env_name])$ conda install [pakage_name(ex:numpy, pandas, scikit-learn, tensorflow-gpu...)]
 ([conda_env_name])$ conda list
```

※ It is recommended to install pages using “conda install” for version management.

※ To use multi-GPUs, both tensorflow-gpu and cudnn, cudatoolkit 10.0 packages need to be installed together.

※ To use multiple GPUs across multiple nodes, refer to \[Appendix 7]: Conda Environment-based Horovod-TensorFlow Installation Method.



◦ Deactivate the conda environment

```
([conda_env_name])$ conda deactivate
```

{% hint style="info" %}
2021년 12월 2일에 마지막으로 업데이트되었습니다.
{% endhint %}
