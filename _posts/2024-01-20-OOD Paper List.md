---
layout: post
toc: true
title: "OOD Paper List"
categories: ML
comments: true
tags: [ML,OOD]
author: 
  - Tingyu Song 
---

## OOD Paper List 

### 1 Read

1. [Is Fine-tuning Needed? Pre-trained Language Models Are Near Perfect for Out-of-Domain Detection](https://arxiv.org/abs/2305.13282) （ACL-2023) 
	* **BG**：主要对目前许多人针对LLM进行FT从而实现OOD Detection的问题进行探究。作者提出LLM本身就是很好的zero-shot的分类器。因而作者主要使用**RoBERTa**对**OoD shift**（x来自不同的数据集）以及**same domain shift**（主要针对同一个数据集进行拆分）进行了探究，同时与FT的结果进行比较。
	* **Results**（1）LLM对于out-of-domain shift表现较好，但对于same domain shift表现较差。（2）在CV里用的sub-probe方式其实在NLP领域中的表现并不是最优解。
	* **Contribution**在于（1）是第一个使用LLM进行zero-shot OOD Detection的，发现zero-shot的表现已经很好；（2）探究了对LLM的FT策略。

2. [GLUE-X: Evaluating Natural Language Understanding Models from an Out-of-Distribution Generalization Perspective]() （ACL-23-findings）提了一个测试OOD在NLP领域的标准，感觉contributions不是特别大。

3. [On Continual Model Refinement in Out-of-Distribution Data Streams]() (ACL-2022)
	* **BG**: 已经部署了的模型在一些OOD上的数据通常很难表现较好。原有的方法主要关注一些特定的简单任务（如手写数据集识别），并且所关注的数据分布较为简单。因而作者主要想探究CL Method在这种OOD的情况下的表现，贡献在于（1）提出了一些新的metrics去衡量CL Method的表现；（2）提了一种产生这些数据的方法（即又包含OOD又包含ID的方法）
	* **Method:** （1）四个Metric主要包括EFR（模型多大程度修复了自己的错误），UKR & OKR (模型多大程度记住了旧知识和新知识)，CSR（持续保持正确的概率），KG（泛化性能）；（2）生成复杂数据的方法主要使用了3个参数，$\alpha$ 指代in-distribution data的占比, $\beta$ 指代包括了某一种特定种类的ood的占比（由Markov链实现）, $\gamma$指代剩下的其他的OOD的数据占比。

4. [feed Two Birds with One Scone: Exploiting Wild Data for Both Out-of-Distribution Generalization and Detection]() 这篇好像很经典，是提出WILDS数据集的论文，在此不过多赘述。

5. [Out-of-distribution Detection Learning with Unreliable Out-of-distribution Sources]() （NeurIPS-2023)
	* **BG & Motivation**: 目前的Data Augumentation方法产生的数据有2个问题，一个是很难和真实世界中的OOD数据相对应，**另一个是和In Distribution存在语义上的重叠**。因而作者提出能否**利用**已有的Data Augumentation Method所生成的数据设计一个辅助的任务，并且这个辅助的任务能够对真正的OOD问题有作用，从而达到前面提到的第二个问题。
	* **Method**:作者的方法主要在于如何解决这两个问题，一个是如何设计这样的一个辅助的任务，另一个是这样的辅助任务为什么对真正的OOD任务是有效的。作者的核心思想在于首先使用一个GAN（或者类似的）去生成 auxiliary OOD 和 auxiliary ID, 并且使用了一些数学公式保证产生的auxiliary OOD 和 auxiliary ID 是完全不重合的。并且生成的auxiliary ID 和true ID 的分布是相同的。（可以看出这个Idea非常简单有效，但是需要很多数学上的condition来保证）
	* **Limitations:** 一方面是需要同时训练Generator和Predictor，另一方面是Generator所生成的数据的多样性与最终表现密切相关（但是作者并没有探究这个问题）。

6. [ODS: Test-Time Adaptation in the Presence of Open-World Data Shift](https://proceedings.mlr.press/v202/zhou23e.html) (ICML-2023)
* **BG & Motivation：** 目前已有的ODS算法主要解决的是covariate shift（输入的分布发生变化）。但在现实生活中输出可能也会发生变化（label shift）。作者主要针对解决covariate shift和label shift同时存在的情况。
* **Method**: 作者用了两个模块。一个是$M_T$用于检测Distribution的shift，另一个是$M_O$用于对Model Prediction进行修正。其中$M_T$的输入是开始的model $f_{\theta}$, 以及第t个时间步改变的$f_{\theta_t}$ 以及输入的samples，输出一个label distribution。而$M_O$模块根据$M_T$输出的label distribution，对$f_{\theta_t}$ 的输出进行修正。之后输出修正后的结果给模型。
* **Results:** 主要使用ResNet18在CIFAR10-C上进行了测试。CIFAR-C 主要是对于图像进行不同程度的模糊。(其实这里有个很好的问题，目前的OOD数据集都只有covariate shift，如何构建具有label shift的数据集呢？作者用了tweak-one shift method去构建，设置了一个$\gamma$作为一种或多种corruption的出现次数)

7. [AdaNPC](https://arxiv.org/abs/2304.12566) （ICML-2023）
	* **BG：** 主要针对目前已有TTA方法的2个不足。（1）需要额外的Domain的信息；（2）需要对model进行FT或者需要额外的model进行训练。
	* **Method：** 应用在TTA阶段。主要利用KNN的方式，对训练数据进行unsupervised train，并存储在数据库中。 之后在inference time时，将数据与数据库中的数据对比，选出最相近的一条。并在给出Prediction之后，若该条的置信度大于某个阈值，将该条数据也加入数据库。
	* **Results**: 在测试的几个Dataset上都实现了SOTA。

8. [SIMPLE: SPECIALIZED MODEL-SAMPLE MATCHING FOR DOMAIN GENERALIZATION]() (ICLR-2023) 主要采用集成学习方法去解决OOD问题，做的是匹配模型和数据的一个新的方法（使用了无监督的方法是去训练）。这个方法有2个问题，一个是集成学习的效率很差，这篇文章使用了283个模型去做，虽然最终使用sub-probe的方法，只用inference 1次，但对于较大的数据集而言，整个时间仍然较久；另一个问题是，这种模型和数据匹配的方法其实很有可能并不make sense，只是从直觉上而言work。

### 2 UnRead 

[Learning to Augment Distributions for Out-of-distribution Detection]()

[Data-OOB: Out-of-bag Estimate as a Simple and Efficient Data Value]()

[Out-of-Distribution Detection and Selective Generation for Conditional Language Models]()

[Diverse Weight Averaging for Out-of-Distribution Generalization]()