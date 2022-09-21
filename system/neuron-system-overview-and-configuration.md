# System Overview and Configuration

## A. Purpose of System Operation

\* It was decided that No.5 system would be a Knights Landing-based system. Consequently, a GPU-based system is operated to accommodate various user demands.

\* Large capacity memory nodes are operated to meet the demand of jobs requiring large memory.

## B. System Configuration

**○ Compute nodes**

**- GPU nodes**

![](../.gitbook/assets/gpu\_nodes.png)

![](../.gitbook/assets/gpu\_nodes2.png)

![](../.gitbook/assets/gpu\_nodes3.png)

**- CPU\_only nodes**

![](../.gitbook/assets/cpu\_only\_nodes.png)

**- High-capacity memory nodes**

![](../.gitbook/assets/high\_capacity\_memory\_nodes.png)

**- Next-generation architecture test nodes**

![](../.gitbook/assets/next\_generation\_architecture\_test\_nodes.png)

**○ Storage configuration**

Neuron storage is a DDN SFA12K-40 model and consists of 10 Enclosure SS8460 (Disk Box) and 10 Oracle X3-2L servers (2 MDS, 8 OSS). The Neuron storage is configured using a Lustre parallel distributed file system, and the actual available capacity is about 2.3 PB. The home directory (/home01), application directory (/apps), and scratch directory (/scratch2) are configured separately in a single file system and are mounted on login and compute nodes.

![\[ Neuron storage configuration diagram\]](<../.gitbook/assets/Neuron 스토리지 구성도.png>)

![](../.gitbook/assets/parallel\_file\_system\_file\_system\_lustre\_2\_10\_5.png)

{% hint style="info" %}
2021년 12월 1일에 마지막으로 업데이트되었습니다.
{% endhint %}
