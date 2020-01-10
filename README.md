# DeepSniffer
DeepSniffer is a model extraction framework that predicts the model architecture of the victim models based on the architecture hints during their execution. Specifically, this project mainly provides the most important part: layer sequence prediction. The key concept of DeepSniffer is to transform the layer sequence to a sequence-to-sequence prediction problem.
# Installation
1) Install the Tensorflow v1.13, Pytorch v0.4.1, and Python 3.6.2.
2) Download the model checkpoint files from the [google drive](https://drive.google.com/drive/folders/1JrTkT9C0klWFMK4x-KSMqvvPJ7k3TL6U?usp=sharing).
# Workflow
This project comprises of two parts: 1) Model extraction part: we provide the source code and data set for training and testing the layer sequence predictor which is the fundamental step for model extraction. 
2) Adversarial attack example: In the further step, we also provide the source code and trained substitute model checkpoints to evaluate the effectiveness of the extracted models on adversarial attacks. 
## Model Extraction
#### Layer Sequence Predictor Inference 
* **Predictors**: We provide the trained layer sequence predictor in /DeepSniffer/ModelExtraction/validate_deepsniffer/predictors， which can be used for predicting the layer sequence of the victim models with their architecture hints. 
* **Dataset**: We provide architecture hint feature file of several commonly-used DNN models (profiling on K40), in the following directory: DeepSniffer/ModelExtraction/dataset/typicalModels.
* **Scripts**: To infer the layer sequence of these victim models, run 
DeepSniffer/ModelExtraction/scripts/.infer_predictor_typicalmodels.sh. The results log files are stored in DeepSniffer/Results/Table4/logs. Run DeepSniffer/Results/Table4/logs/results_analysis.py to display the final prediction error rate results.

#### Layer Sequence Predictor Training
* **Dateset**: We randomly generate computational graphs and profile the GPU performance counter information (kernel latency, read volume, write volume) during their execution to train the layer sequence predictor. The training and testing dataset is in the following directory: DeepSniffer/ModelExtraction/dataset/training_randomgraphs.

* **Scripts**: To train the layer sequence predictor for model extraction, run DeepSniffer/ModelExtraction/scripts/train_predictor.sh. The trained model is in the directory of DeepSniffer/ModelExtraction/training_deepsniffer and training log is under the model checkpoint file directory.

* **Results**: The training log files are in the following directory: DeepSniffer/Results/Figure6/logs.

## Adversarial Attack with DeepSniffer
We show an example of targeted adversarial attack on ResNet18 (Golden model). DeepSniffer adopts the extracted neural network architecture to build the substitute models. For comparison, the baseline examines the substitute models established from following networks: VGG family, ResNet family, DenseNet family, SqueezeNet, and Inception.

* **Substitute model checkpoints**: The checkpoints for the trained substitute model can be downloaded from [google drive](https://drive.google.com/drive/folders/1JrTkT9C0klWFMK4x-KSMqvvPJ7k3TL6U?usp=sharing). Unzip the files in the ''models'' to the directory of  DeepSniffer/AdversarialAttack/. 
* **Datasets**: We provide a smaller dataset from ImageNet for adversarial attack experiments, which can be downloaded from [google drive](https://drive.google.com/drive/folders/1JrTkT9C0klWFMK4x-KSMqvvPJ7k3TL6U?usp=sharing). Unzip the files in ''data_100'' under the directory of DeepSniffer/AdversarialAttack/data_100. 
To reproduce the results of Table6, run python DeepSniffer/AdversarialAttack/AdversarialAttack.py. To reproduce the results of Figure 10, run python DeepSniffer/AdversarialAttack/auto_attack_labels_random.py. 
* **Results**: The log files of attack results locate under the directory of DeepSniffer/AdversarialAttack/attack_result/, run DeepSniffer/AdversarialAttack/attack_result/parser_ae.py to display the attack success rate of different substitute models.


**If you have any questions, please feel free to open issue in this project. Thanks!**


