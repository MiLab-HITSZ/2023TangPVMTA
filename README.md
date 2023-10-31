# PVMTA: Prediction Vector Monitoring and Adaptive Temperature Adjustment Against Model Extraction

Implementation of paper *"Y. Tang, W, Luo and Z. Ye, et. al., PVMTA: Prediction Vector Monitoring and Adaptive Temperature Adjustment Against Model Extraction"*

# Requirements

* Python 3.8.12
* Pytorch 1.10.0

The environment can be set up as:
```bash
$ pip install -r requirements.txt       # pip
```

# Usage

### If you want to train the defender model

- You can run the following prompt to train the defender model:
    > python src/defender.py --dataset=fashionmnist --model_tgt=res20_mnist --epochs=100 

- The dataset can be selected from {"kmnist", "mnist", "cifar10", "cifar10_gray", "cifar100", "svhn", "gtsrb", "fashionmnist", "fashionmnist_32", "mnist_32"}

- The model(model_tgt) can be selected from {"res20", "res20_mnist", "res20d", "res20d_cifar", "res20p", "res20p_cifar", "res18_ptm", "vgg13_bn"}, in which the {"res20", "res18_ptm", "vgg13_bn"} are trained for non-gray dataset without defense ("res20_mnist" for gray dataset), the {"res20d", "res20p"} are trained for non-gray dataset with the defense of {PVMTA, PRADA} respectively, and the {"res20d_cifar", "res20p_cifar"} for gray dataset.

- The epochs is set for adjusting the training epochs of defender model.

- Especially, if you want to train a defender model based on the defense AM, you can run the following prompt (the model_tgt can be chosen from {"res20a_mnist", "res20a"} for gray dataset and non-gray dataset respectively):
    > python src/defender_AM.py --dataset=fashionmnist --model_tgt=res20a_mnist --epochs=100


### Launch the attack

- After training a defender model (which will be stored in logs folder), you can lauch the attack between "Knockoff" and "MAZE". You can lauch the attack by running the following prompt:

    > python src/attacker.py --dataset=fashionmnist --model_tgt=res20_mnist --model_clone=wres22 --attack=maze --budget=3e7 --lr_clone=0.1 --lr_gen=1e-3 --iter_clone=5 --iter_exp=10

    > python src/attacker.py --dataset=fashionmnist --model_tgt=res20_mnist --model_clone=wres22 --attack=knockoff --dataset_sur=mnist --epochs=100

- You can change the dataset_sur (which can be selected from the above datasets) to lauch the Knockoff, also you can change the budget of MAZE to control its allowed query counts. To attack different defender models, you can change the model_tgt, which has been decided when you train it. As for the MAZE attack, you can also change the hyper-parameters lr_clone, lr_gen, iter_clone, iter_exp 
to change its setting.

- Especially, for AM defender model, you can lauch the attack by running the following prompt:
    > python src/attacker_AM.py --dataset=fashionmnist --model_tgt=res20a_mnist --model_clone=wres22 --attack=knockoff --dataset_sur=mnist --epochs=100
