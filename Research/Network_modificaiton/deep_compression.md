## Deep Compression: Compressing deep neural networks with pruning, trained quantization and huffman coding

Authors: Song Han, Huizi Mao, Wiliam J. Dally

Link: https://arxiv.org/pdf/1510.00149.pdf

### Abstract
We introduce 'deep compression', a three stage pipeline: pruning, trained quantization and Huffman coding, to reduce storage by 35x to 49x without affecting accuracy. First, the method learns only the important connections. Next, we quantize the weights to enforce weight sharing. Finally, we apply Huffman coding. After the first two steps we retrain the network to fine tune the remaining connections and the quantized centroids. Pruning, reduces the number of connections by 9x to 13x; Quantization then reduces the number of bits that represent each connection from 32 to 5.
On the ImageNet dataset, out method reduced the storage required by AlexNet by 35x, from 240MB to 6.9MB, without loss of accuracy. Our method reduced the size of VGG-16 by 49x from 552MB to 11.3MB, with no loss of accuracy. This allows fitting the model into on-chip SRAM cache rather than off-chip DRAM memory. Our compression method also facilitates the use of complex neural networks in mobile applications where application size and download bandwidth are constrained. Benchmarked on CPU, GPU and mobile GPU, compressed network has 3x to 4x layerwise speedup and 3x to 7x better energy efficiency.

