# How to Use Lustre Striping

## A. Basic Settings for the Neuron Lustre Striping

\
The Neuron Lustre File system supports file striping. Therefore, a single file was distributed and stored in multiple object storage targets (OST), i.e., several physically distributed disks. Consequently, it was possible to reduce bottlenecks and improve input/output (I/O) performance. In particular, the progressive file layout (PFL), which is supported by Lustre 2.10, is applied to the /scratch file system. This function automatically applies the number of stripe-counts according to the file size, without the separate striping setting of the user, thereby improving the I/O performance. The striping settings of the Neuron file system are presented as follows.

![](../.gitbook/assets/basic\_settings\_for\_the\_neuron\_lustre\_striping.png)

&#x20;****&#x20;

## B. Lustre Striping Concept

![Figure 1 The Concept of Lustre Striping
(사용자에게 보여지는 파일: Files visible to the user
Stripe에 의해 분할: Partitioned by the stripe
분할된 파일은 RAID에 의해 한번 더 분할되어 저장: Partitioned files further split by the RAID and stored)](../.gitbook/assets/ByuiN89DGA7hjDU.png)

Lustre partitions data for each OST to maximize the I/O performance of large files. The maximum number of partitions for which parallelization is valid is equal to the OST number. Figure 1 shows how a single file can be parallelly stored in the OST using the Lustre striping function.

## C. Stripe Settings and Verification

```
$ lfs setstripe [--stripe-size|-s size] [--stripe-count|-c count] filename|dirname
```

\- The command applies the striping settings to a file or directory. The striping settings are applied to files created with the aforementioned command, or all the files created in the directory, to which the aforementioned command is applied.

◦--stripe-size

∙Sets the size of the data to be stored in each OST.

∙Stores data in the next OST when the data of the specified size has been stored.

∙The default value is 1 MB, which is used if the stripe\_size is set to 0. The stripe\_size must be a multiple of 64 KB; moreover, it must be less than 4 GB.

◦--stripe-count

∙Sets the number of OSTs to be adopted in striping.

∙The default value is 1; the default value is used if the stripe\_count is set to 0.

∙All possible OSTs are used if the stripe\_count is -1.

```
$ lfs getstripe filename|dirname
```

\- The command checks the values of the striping settings that have been applied to a file or directory.

## D. Recommended Guidelines and Tips

◦ If setstripe is specified in the job script for the directory where the result file of the model will be saved, all the subdirectories and files that are subsequently created inherit this setting value.

◦ When --stripe-count is set to 4 for files larger than or equal to 1 GB, performance is enhanced in most cases. If a larger value is adopted, it needs to be tested.

◦ --stripe-size is only valid when the file size is several TB or larger; hence, it is acceptable to use the default value in most cases.

{% hint style="info" %}
2021년 12월 14일에 마지막으로 업데이트되었습니다.
{% endhint %}
