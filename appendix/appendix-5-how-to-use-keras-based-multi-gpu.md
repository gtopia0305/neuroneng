# How to Use Keras-Based Multi-GPU

Keras is an open-source neural network library written in the Python programming language; it is a high-level neural network application programming interface (API) that runs on MXNet, Deeplearning4j, TensorFlow, Microsoft Cognitive Toolkit, or Theano. In the ivy\_k40\_2, ivy\_v100\_2, cas\_v100\_2, cas\_v100nv\_4, and cas\_v100nv\_8 queues of the Neuron system, each node is equipped with 2, 4, or 8 GPUs. Hence, the environment is set up to train neural networks using multiple GPUs, even when a single node is adopted.

◦ Method for changing the code and submitting a job to employ Multi-GPU

1\) \[from keras.utils import multi\_gpu\_model] Add module

```
import keras
 from keras.datasets import cifar10
 from keras.models import Sequential
 from keras.layers import Dense, Dropout, Activation, Flatten
 from keras.layers import Conv2D, MaxPooling2D
 from keras.utils import multi_gpu_model
 import os
```

2\) Declare the use of Multi-GPU in the code

```
# initiate RMSprop optimizer
 opt = keras.optimizers.rmsprop(lr=0.0001, decay=1e-6)
 # multi-gpu
 model = multi_gpu_model(model, gpus=2)
 # Let's train the model using RMSprop
 model.compile(loss='categorical_crossentropy', optimizer=opt, metrics=['accuracy'])
```

\- Set GPUs according to the number of GPUs to be used. (ex. set GPUs = 4 in the as\_v100nv\_4 node case)

3\) Job submission script

```
#!/bin/sh
 #SBATCH -J keras
 #SBATCH --time=24:00:00
 #SBATCH -o %x_%j.out
 #SBATCH -e %x_%j.err
 #SBATCH -p ivy_v100_2
 #SBATCH --comment tensorflow
 #SBATCH --gres=gpu:2
 #SBATCH -N 1
 
 module purge
 module load  gcc/8.3.0 cuda/10.0 cudampi/openmpi-3.1.0 conda/tensorflow_1.13
 
 srun python example.py
```
