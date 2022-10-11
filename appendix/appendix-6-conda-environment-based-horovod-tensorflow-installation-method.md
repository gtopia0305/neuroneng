# Conda Environment-Based Horovod-TensorFlow Installation Method

Horovod adopts a standard message passing interface (MPI) model to send messages and manage the communication between nodes in a high-performance distributed computing environment. The Horovod MPI implementation provides a more simplified programming model than the standard TensorFlow distributed training model. To train a training model using multiple nodes based on the conda environment in the Neuron system, Horovod and required libraries can be installed, according to the following method, and the model is subsequently executed.

\- For information on how to use Horovod, refer to \[Appendix 8].



1\) Create a conda environment

```
 $ module load gcc/8.3.0 cuda/10.0 cudampi/openmpi-3.1.0 python/3.7.1
 $ conda create -n my_tensorflow
 $ source activate my_tensorflow
 (my_tensorflow)  $
```

\- For detailed information on how to use conda, refer to \[Appendix 5].



2\) Install and link the required libraries

```
 (my_tensorflow) $ conda install tensorflow-gpu python=3.7.1 cudnn cudatoolkit=10.0
 (my_tensorflow) $ cd $HOME/.conda/envs/my_tensorflow/lib
 (my_tensorflow) $ ln -s /apps/applications/miniconda3/glibc-2.29/lib/libm-2.29.so libm.so.6
 (my_tensorflow) $ cp /apps/cuda/10.0/lib64/libnccl* .
 (my_tensorflow) $ cd ../include
 (my_tensorflow) $ cp /apps/cuda/10.0/include/nccl* .
```

※ ln -s: command for creating a symbolic link



3\) Install Horovod

```
 (my_tensorflow) $ HOROVOD_NCCL_HOME=$HOME/.conda/envs/my_tensorflow
 (my_tensorflow) $ HOROVOD_CUDA_HOME=/apps/cuda/10.0 
 (my_tensorflow) $ HOROVOD_GPU_ALLREDUCE=NCCL
 (my_tensorflow) $ HOROVOD_WITH_TENSORFLOW=1 
 (my_tensorflow) $ pip install horovod==0.18.2
```

{% hint style="info" %}
2021년 12월 2일에 마지막으로 업데이트되었습니다.
{% endhint %}
