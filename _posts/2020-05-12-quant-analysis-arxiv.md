---
layout: blogpost
title: Theo Olausson
subtitle: arXiv Preprint | Quantitative Analysis of Image Classification Techniques for Memory-Constrained Devices
frontimg: /assets/multi-fastgrnn-web.jpg
topics: [artificial-intelligence, machine-learning, paper]
---
<p>
Super happy to share that today some of my fellow students and I put up a preprint
on arXiv. <a href="https://arxiv.org/abs/2005.04968">Check it out here!</a>
The paper consists of a thorough comparison of image classification techniques for
memory-constrained devices, which is an interesting field since it has not yet converged
to the same extent that ML more broadly has. For example, nobody in their right mind
would probably even consider using anything other than a
convolutional neural network (CNN) for image classification in general,
but things are not so clear cut when memory footprint is an important factor to consider.
</p>
<p>
Thanks to Sebastian and John for working with me on this preprint, and to Pavlos for supervising the project!
</p>
<h3> Abstract </h3>
<p>
Convolutional Neural Networks, or CNNs, are the state of the art for image classification, but typically come at the cost of a large memory footprint. This limits their usefulness in applications relying on embedded devices, where memory is often a scarce resource. Recently, there has been significant progress in the field of image classification on such memory-constrained devices, with novel contributions like the ProtoNN, Bonsai and FastGRNN algorithms. These have been shown to reach up to 98.2% accuracy on optical character recognition using MNIST-10, with a memory footprint as little as 6KB. However, their potential on more complex multi-class and multi-channel image classification has yet to be determined. In this paper, we compare CNNs with ProtoNN, Bonsai and FastGRNN when applied to 3-channel image classification using CIFAR-10. For our analysis, we use the existing Direct Convolution algorithm to implement the CNNs memory-optimally and propose new methods of adjusting the FastGRNN model to work with multi-channel images. We extend the evaluation of each algorithm to a memory size budget of 8KB, 16KB, 32KB, 64KB and 128KB to show quantitatively that Direct Convolution CNNs perform best for all chosen budgets, with a top performance of 65.7% accuracy at a memory footprint of 58.23KB.
</p>
