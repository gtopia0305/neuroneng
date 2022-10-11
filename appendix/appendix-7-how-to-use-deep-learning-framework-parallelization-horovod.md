# How to Use Deep Learning Framework Parallelization (Horovod)

## A. How to Use Horovod in TensorFlow

It is possible to parallelize by linking Horovod with TensorFlow when adopting multiple GPUs across multiple nodes. As presented in the following example, Horovod can be linked with TensorFlow by introducing a code for using Horovod. Both TensorFlow and all the Keras APIs that can be adopted in TensorFlow can be linked with Horovod. First, we introduce how to link Horovod with TensorFlow.\
(Example: MNIST Dataset and LeNet-5 CNN structure)



※ Refer to the official Horovod guide for detailed information on how to link Horovod with TensorFlow.\
([https://github.com/horovod/horovod#usage](https://github.com/horovod/horovod#usage))



◦ The import statement for linking Horovod with TensorFlow and the Horovod initialization in the main function

```
 import horovod.tensorflow as hvd
 ...
 hvd.init()
```

※ horovod.tensorflow: a module for linking Horovod with TensorFlow

※ Horovod is initialized; hence, it can be used.



◦ Set the dataset to use Horovod in the main function

```
 (x_train, y_train), (x_test, y_test) = \
 keras.datasets.mnist.load_data('MNIST-data-%d' % hvd.rank())
```

※ The dataset to be accessed for each job is set and created according to the Horovod rank.



◦ Set the Horovod-related settings, broadcast, and number of training epochs for the optimizer in the main function

```
 opt = tf.train.AdamOptimizer(0.001 * hvd.size())
 opt = hvd.DistributedOptimizer(opt)
 global_step = tf.train.get_or_create_global_step()
 train_op = opt.minimize(loss, global_step=global_step)
 hooks = [hvd.BroadcastGlobalVariablesHook(0),
 tf.train.StopAtStepHook(last_step=20000 // hvd.size()), ... ]
```

※ Apply Horovod-related settings to the optimizer and use broadcast to convey them to each job.

※ Set the training process step of each job according to the number of Horovod jobs.

◦ Allocate GPU devices according to the Horovod process rank

<pre><code><strong> config = tf.ConfigProto()
</strong> config.gpu_options.allow_growth = True
 config.gpu_options.visible_device_list = str(hvd.local_rank())</code></pre>

※ Allocate a single job for each GPU according to the Horovod local rank.



◦ Set checkpoint for the rank 0 job

```
 checkpoint_dir = './checkpoints' if hvd.rank() == 0 else None
 ...
 with tf.train.MonitoredTrainingSession(checkpoint_dir=checkpoint_dir,
 hooks=hooks,
 config=config) as mon_sess:
```

※ The job that involves saving or retrieving a checkpoint must be carried out by a single process; hence, it is set to rank 0.

## B. How to Use Horovod in Keras

By linking Keras with Horovod, parallelization is possible even when Keras APIs are adopted in TensorFlow. As shown in the following example, Horovod can be linked with Keras by introducing a code for using Horovod.\
(Example: MNIST Dataset and LeNet-5 CNN structure)



※ Refer to the official Horovod guide for detailed information on how to link Horovod with Keras.\
([https://github.com/horovod/horovod/blob/master/docs/keras.rst](https://github.com/horovod/horovod/blob/master/docs/keras.rst))



◦ The import statement for linking Horovod with Keras and the Horovod initialization in the main function

<pre><code><strong> import horovod.tensorflow.keras as hvd
</strong> ...
 hvd.init()</code></pre>

**※ horovod.tensorflow.keras: a module for using Horovod with Keras in TensorFlow**

※ Horovod is initialized; hence, it can be used.



◦ Allocate GPU devices according to the Horovod process rank

<pre><code><strong> config = tf.ConfigProto()
</strong> config.gpu_options.allow_growth = True
 config.gpu_options.visible_device_list = str(hvd.local_rank())</code></pre>

※ Allocate a single job for each GPU according to the Horovod local rank.



◦ Set the Horovod-related settings, broadcast, and number of training epochs for the optimizer in the main function

```
 epochs = int(math.ceil(12.0 / hvd.size()))
 ...
 opt = keras.optimizers.Adadelta(1.0 * hvd.size())
 opt = hvd.DistributedOptimizer(opt)
 callbacks = [ hvd.callbacks.BroadcastGlobalVariablesCallback(0), ]
```

※ Set the training process step of each job according to the number of Horovod jobs.

※ Apply Horovod-related settings to the optimizer and use broadcast to convey them to each job.



◦ Set checkpoint for the rank 0 job

```
if hvd.rank() == 0:
     callbacks.append(keras.callbacks.ModelCheckpoint('./checkpoint-{epoch}.h5'))
```

※ The job that involves saving or retrieving a checkpoint must be performed by a single process; hence, it is set to rank 0.



◦ Allocate GPU devices according to the Horovod process rank

```
model.fit(x_train, y_train, batch_size=batch_size, callbacks=callbacks, epochs=epochs, verbose=1 if hvd.rank() == 0 else 0, validation_data=(x_test, y_test))
```

※ To print phrases that are solely output during training from the rank 0 job, the verbose value is set to 1 for the rank 0 job alone.

## C. How to Use Horovod in PyTorch

It is possible to parallelize by linking Horovod with PyTorch when employing multiple GPUs across multiple nodes. As presented in the following example, Horovod can be linked with PyTorch by introducing a code for using Horovod.\
(Example: MNIST Dataset and LeNet-5 CNN structure)



※ Refer to the official Horovod guide for detailed information on how to use Horovod in PyTorch.\
([https://github.com/horovod/horovod/blob/master/docs/pytorch.rst](https://github.com/horovod/horovod/blob/master/docs/pytorch.rst))



◦ The import statement for linking Horovod with PyTorch and the Horovod initialization in the main function

```
 import torch.utils.data.distributed
 import horovod.torch as hvd
 ...
 hvd.init()
 if args.cuda:
     torch.cuda.set_device(hvd.local_rank())
     torch.set_num_threads(1)
```

※ torch.utils.data.distributed: module for performing distributed training in PyTorch

※ horovod.torch: module for adopting Horovod with PyTorch

※ Horovod is initialized, and the device that will execute the job is set according to the rank that was set in the initialization process.

※ To use one CPU thread for each job, torch.set\_num\_threads(1) is employed.



◦ Add Horovod-related information in the training process

<pre><code><strong> def train(args, model, device, train_loader, optimizer, epoch):
</strong> ...
 train_sampler.set_epoch(epoch)
 ...
     if batch_idx % args.log_interval == 0:
         print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(
                 epoch, batch_idx * len(data), len(train_sampler),
                 100.* batch_idx / len(train_loader), loss.item()))</code></pre>

※ train\_sampler.set\_epoch(epoch): sets the train sampler’s epoch

※ Because the training dataset is split across and processed by several jobs, len(train\_sampler) is adopted to verify the total dataset size.



◦ Calculate the average value using Horovod

```
 def metric_average(val, name):
 tensor = torch.tensor(val)
 avg_tensor = hvd.allreduce(tensor, name=name)
 return avg_tensor.item()
```

※ The average value is calculated using the Allreduce communication method of Horovod to calculate the average value across several nodes.



◦ Add Horovod-related information in the test process

```
 test_loss /= len(test_sampler)
 test_accuracy /= len(test_sampler)
 test_loss = metric_average(test_loss, 'avg_loss')
 test_accuracy = metric_average(test_accuracy, 'avg_accuracy')
 if hvd.rank() == 0:
     print('\nTest set: Average loss: {:.4f}, Accuracy: {:.2f}%\n'.format(
            test_loss, 100. * test_accuracy))
```

**※ The metric\_average function presented above is adopted because the calculation of the average value is required across several nodes.**

※ Because each node has the same calculated values for loss and accuracy via the Allreduce communication, rank 0 executes the print function.



◦ Set dataset to use Horovod in the main function

<pre><code><strong> train_dataset = datasets.MNIST('data-%d' % hvd.rank(), train=True, download=True,
</strong> transform=transforms.Compose([transforms.ToTensor(),
 transforms.Normalize((0.1307,), (0.3081,)) ]))
 train_sampler = torch.utils.data.distributed.DistributedSampler(
 train_dataset, num_replicas=hvd.size(), rank=hvd.rank())
 train_loader = torch.utils.data.DataLoader(
 train_dataset, batch_size=args.batch_size, sampler=train_sampler, **kwargs)
 test_dataset = datasets.MNIST('data-%d' % hvd.rank(), train=False, transform=transforms.Compose([
 transforms.ToTensor(), transforms.Normalize((0.1307,), (0.3081,)) ]))
 test_sampler = torch.utils.data.distributed.DistributedSampler(
 test_dataset, num_replicas=hvd.size(), rank=hvd.rank())
 test_loader = torch.utils.data.DataLoader(test_dataset, batch_size=args.test_batch_size,
 sampler=test_sampler, **kwargs)</code></pre>

※ The dataset to be accessed for each job is set and created according to the Horovod rank.

※ Set the distributed sampler of PyTorch and assign it to the data loader.



◦ Add Horovod-related settings to the optimizer and the sampler to the training and test process in the main function

```
 optimizer = optim.SGD(model.parameters(), lr=args.lr * hvd.size(), momentum=args.momentum)
 hvd.broadcast_parameters(model.state_dict(), root_rank=0)
 hvd.broadcast_optimizer_state(optimizer, root_rank=0)
 optimizer = hvd.DistributedOptimizer(optimizer, named_parameters=model.named_parameters())
 for epoch in range(1, args.epochs + 1):
     train(args, model, train_loader, optimizer, epoch, train_sampler)
     test(args, model, test_loader, test_sampler)
```

※ Apply Horovod-related settings to the optimizer and use broadcast to convey them to each job.

※ Add the sampler to the training and test processes, and pass it to each function.

{% hint style="info" %}
2021년 12월 2일에 마지막으로 업데이트되었습니다.
{% endhint %}
