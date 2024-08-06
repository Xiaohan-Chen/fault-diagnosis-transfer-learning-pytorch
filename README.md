# Deep transfer learing for fault diagnosis

## :book: 1. Introduction
This repository contains popular deep transfer learning algorithms implemented via PyTorch for cross-load fault diagnosis transfer tasks, including:  

- [x] General supervised learning classification task: traing and test apply the same machines, working conditions and faults.

- [x] *domain adaptation*: the distribution of the source domain data may be different from the target domain data, but the label set of the target domain is the same as the source domain, i.e., $\mathcal{D} _{s}=(X_s,Y_s)$, $\mathcal{D} _{t}=(X_t,Y_t)$, $X_s \ne X_t$, $Y_s = Y_t$.
  - [x] **DDC**: Deep Domain Confusion [[arXiv 2014]](https://arxiv.org/pdf/1412.3474.pdf)
  - [x] **Deep CORAL**: Correlation Alignment for Deep Domain Adaptation [[ECCV 2016]](https://arxiv.org/abs/1607.01719)
  - [x] **DANN**: Unsupervised Domain Adaptation by Backpropagation [[ICML 2015]](http://proceedings.mlr.press/v37/ganin15.pdf)
  - [ ] TODO

- [x] *Open-set domain adaptation*: the distribution of the source domain data may be different from the target domain data. What's more, the target label set contains unknown categories, i.e., $\mathcal{D} _{s}=(X_s,Y_s)$, $\mathcal{D} _{t}=(X_t,Y_t)$, $X_s \ne X_t$, $Y_s \in Y_t$. We refer to their common categories $\mathcal{Y}_s\cap \mathcal{Y}_t$ as the *known classes*, and $\mathcal{Y}_s\setminus \mathcal{Y}_t$ (or $\mathcal{Y}_t\setminus \mathcal{Y}_s$) in the target domain as the *unknown class*.
  - [x] **OSDABP**: Open Set Domain Adaptation by Backpropagation [[ECCV 2018]](http://openaccess.thecvf.com/content_ECCV_2018/papers/Kuniaki_Saito_Adversarial_Open_Set_ECCV_2018_paper.pdf)
  - [ ] TODO

> **Few-shot** learning-based bearing fault diagnosis methods please see: https://github.com/Xiaohan-Chen/few-shot-fault-diagnosis

## :balloon: 2. Citation

For further introductions to transfer learning in bearing fault diagnosis, please read our [paper](https://ieeexplore.ieee.org/document/10042467). And if you find this repository useful and use it in your works, please cite our paper, thank you~:
```
@ARTICLE{10042467,
  author={Chen, Xiaohan and Yang, Rui and Xue, Yihao and Huang, Mengjie and Ferrero, Roberto and Wang, Zidong},
  journal={IEEE Transactions on Instrumentation and Measurement}, 
  title={Deep Transfer Learning for Bearing Fault Diagnosis: A Systematic Review Since 2016}, 
  year={2023},
  volume={72},
  number={},
  pages={1-21},
  doi={10.1109/TIM.2023.3244237}}
```

---
## :wrench: 3. Requirements
- python 3.9.12
- Numpy 1.23.1
- pytorch 1.12.0
- scikit-learn 1.1.1
- torchvision 0.13.0

---
## :handbag: 4. Dataset
Download the bearing dataset from [CWRU Bearing Dataset Center](https://engineering.case.edu/bearingdatacenter/48k-drive-end-bearing-fault-data) and place the `.mat` files in the `./datasets` folder according to the following structure:
```
datasets/
  └── CWRU/
      ├── Drive_end_0/
      │   └── 97.mat 109.mat 122.mat 135.mat 174.mat 189.mat 201.mat 213.mat 226.mat 238.mat
      ├── Drive_end_1/
      │   └── 98.mat 110.mat 123.mat 136.mat 175.mat 190.mat 202.mat 214.mat 227.mat  239.mat
      ├── Drive_end_2/
      │   └── 99.mat 111.mat 124.mat 137.mat 176.mat 191.mat 203.mat 215.mat 228.mat 240.mat
      └── Drive_end_3/
          └── 100.mat 112.mat 125.mat 138.mat 177.mat 192.mat 204.mat 217.mat 229.mat 241.mat
```

---
## :pencil: 5. Usage
> **NOTE**: When using pre-trained models to initialise the backbone and classifier in transfer learning tasks, run classification tasks first to generate corresponding checkpoints.

Four typical neural networks are implemented in this repository, including MLP, 1D CNN, 1D ResNet18, and 2D ResNet18(torchvision package). More details can be found in the `./Backbone` folder.

**General Supervised Learning Classification:**
- Train and test the model on the same machines, working conditions and faults. Use the following commands:
```python
python3 classification.py --datadir './datasets' --max_epoch 100
```

**Transfer Learning:**
- If using the DDC transfer learning method, use the following commands:
```python
python3 DDC.py --datadir './datasets' --backbone "CNN1D" --pretrained False --kernel 'Linear'
```
- If using the DeepCORAL transfer learning method, use the following commands:
```python
python3 DDC.py --datadir './datasets' --backbone "CNN1D" --pretrained False --kernel 'CORAL'
```
- If using the DANN transfer learning method, use following commands:
```python
python3 DANN.py --backbone "CNN1D"
```
**Open Set Domain Adaptation:**
- The target domain contains unknow classes, use the following commands:
```python
python3 OSDABP.py
```
---
## :flashlight: 6. Results
> The following results do not represent the best results.

**General Classification task:**  
Dataset: CWRU  
Load: 3  
Label set: [0,1,2,3,4,5,6,7,8,9]  

|                   | MLPNet | CNN1D | ResNet1D | ResNet2D |
| :---------------: | :----: | :---: | :------: | :------: |
| acc (time domain) | 93.95  | 97.70 |  99.58   |  98.02   |
| acc (freq domain) | 99.95  | 99.44 |  100.0   |  99.96   |

**Transfer Learning:**  
Dataset: CWRU  
Source load: 3  
Target Load: 2  
Label set: [0,1,2,3,4,5,6,7,8,9]  
Pre-trained model: True  

Time domain:  
|                     | MLPNet | CNN1D | ResNet1D | ResNet2D |
| :-----------------: | :----: | :---: | :------: | :------: |
| DDC (linear kernel) | 75.47  | 85.53 |  91.79   |  91.32   |
|      DeepCORAL      | 82.33  | 88.23 |  93.88   |  90.84   |
|        DANN         | 87.68  | 94.77 |  98.88   |  93.95   |

Frequency domain
|           | MLPNet | CNN1D | ResNet1D | ResNet2D |
| :-------: | :----: | :---: | :------: | :------: |
| DeepCORAL | 98.65  | 98.22 |  99.75   |  99.31   |
|   DANN    | 99.38  | 98.74 |  99.89   |  99.47   |

**Open Set Domain Adaptation**  
- *OSDABP*
Dataset: CWRU  
Source load: 3  
Target Load: 2  
Source label set: [0,1,2,3,4,5]  
Target label set: [0,1,2,3,4,5,6,7,8,9]  
Pre-trained model: True  

|  Label   |   0   |   1   |   2   |   3   |   4   |   5   |  unk  | All   | Only known |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | ----- | ---------- |
|  MLPNet  | 99.83 | 95.96 | 59.76 | 76.10 | 19.85 | 96.58 | 59.21 | 70.21 | 75.99      |
|  CNN1D   | 100.0 | 94.95 | 94.47 | 99.08 | 47.31 | 74.32 | 26.36 | 61.75 | 85.35      |
| ResNet1D | 100.0 | 100.0 | 80.14 | 100.0 | 43.32 | 93.49 | 45.22 | 70.04 | 86.58      |
| ResNet2D | 100.0 | 100.0 | 94.82 | 100.0 | 18.55 | 98.12 | 53.42 | 72.95 | 85.96      |


---
## :camping: 7. See also
- Multi-scale CNN and LSTM bearing fault diagnosis [[paper](https://link.springer.com/article/10.1007/s10845-020-01600-2)][[GitHub](https://github.com/Xiaohan-Chen/baer_fault_diagnosis)]
- TFPred self-supervised learning for few labeled fault diagnosis [[Paper](https://www.sciencedirect.com/science/article/pii/S0967066124000601)][[GitHub](https://github.com/Xiaohan-Chen/TFPred)]

---
## :globe_with_meridians: 8. Acknowledgement

```
@article{zhao2021applications,
  title={Applications of Unsupervised Deep Transfer Learning to Intelligent Fault Diagnosis: A Survey and Comparative Study},
  author={Zhibin Zhao and Qiyang Zhang and Xiaolei Yu and Chuang Sun and Shibin Wang and Ruqiang Yan and Xuefeng Chen},
  journal={IEEE Transactions on Instrumentation and Measurement},
  year={2021}
}
```

I would like to thank the following person for contributing to this repository: [@Wang-Dongdong](https://github.com/Wang-Dongdong),[@zhuting233](https://github.com/zhuting233)