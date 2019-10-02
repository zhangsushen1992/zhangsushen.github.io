## A LSTM and CNN based assemble neural network framework for arrhythmias classification

Author: Fan Liu, Xingshe Zhou, Jinli Cao, et al

Link: https://ieeexplore.ieee.org/abstract/document/8682299

### Abstract
This paper proposes a LSTM and CNN based assemble neural network framework to distinguish different types of arrhythmias by integrating stacked bidirectional LSTM (SB-LSTM) network and two-dimensional CNN (TD-CNN). Particularly, SB-LSTM is used to mine the long-term dependencies contained in electrocardiogram (ECG) from two directions to model the overall variation trends of ECG, while TD-CNN aims at extracting local information of ECG to characterise the local features of ECG. Moreover, we design an ensemble empirical mode decomposition (EEMD) based signal decomposition layer and a support vector machine based intermediate result fusion layer, by which ECG can be analysed more efficiently, and the final classificiation reulsts can be more accurate and robust. The model surpasses state-of-the-art.

### Method
The framework consists of four layers, SDL, SB-LSTM, TD-CNN, FL (fusion layer). Concretely, the ECG signal is first decomposed into multiple intrinsic mode functions (IMFs). Then, the IMFs with lower frequency and higher frequency are fed into SB-LSTM layer and TD-CNN layer, with each IMF obtains an intermediate classification result. Finally, they are fused into a final classification result via FL. At a holistic level, the proposed model resembles a bagging classifier [15-16]. Specifically, SDL is used to create new instances, SB-LSTM and TD-CNN play the role of weak classifiers, and FL is used to fuse the intermediate classification results.
