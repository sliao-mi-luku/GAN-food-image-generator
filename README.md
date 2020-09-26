# GAN-food-image-generator
Use GANs to generate food images (with Kaggle's Food-101 dataset)


**Original paper**

I. J. Goodfellow et al. (2014) **Generative Adversarial Nets** [[arXiv]](https://arxiv.org/abs/1406.2661)

---
## The Dataset
In this repo I used the **Food-101 dataset on Kaggle** [[kaggle dataset]](https://www.kaggle.com/kmader/food41)

Because of my local desktop's memory/computation capacity, I used the subset **food_c101_n10099_r64x64x3.h5**, which consists of 10,999 colored images of size 64x64 pixel^2

To access to the dataset, simply uses the following commands:

``` python3
## Load the subdataset
import h5py
data_dir = 'archive'
filename = 'food_c101_n10099_r64x64x3.h5'

food_dataset = h5py.File(os.path.join(data_dir, filename), 'r')

category = np.array(food_dataset['category'])
category_names = np.array(food_dataset['category_names'])
data = np.array(food_dataset['images'])
```

The dataset consists of 3 keys: `category`, `category_names`, and `data`

`food_dataset['category']` gives the one-hot coded vector of the category

`food_dataset['category_names']` gives the name of the category of the image (for example, 'ceviche')

`food_dataset['data']` gives the pixel values of all images. Shape (10099, 64, 64, 3)

To visualize an image from the dataset:

``` python3
import matplotlib.pyplot as plt

idx = 10

plt.figure()
plt.imshow(data[idx,:,:,:])
plt.title(category_names[idx])
plt.axis('off')
plt.show()
```


---
## Model Atchitecture

In this repo, I implemented 2 models of GANs using different types of layers/architectures.

The first model uses deep fully connected layers.

The second model uses deep deconvolutional layers for the generator, and uses deep convolutional layers for the discriminator.

#### The Generator

The generator consists of 4 fully connected layers:

**Inputs**: an noise vector of dimension (hidden_dim) = 256

**FC1**: 256 units, LeakyReLU activation with alpha = 0.1, and batch normalizaion\
**FC2**: 512 units, LeakyReLU activation with alpha = 0.1, and batch normalizaion\
**FC3**: 512 units, LeakyReLU activation with alpha = 0.1, and batch normalizaion\
**FC4 (output layer)**: h*w*3 units (the size of the fake image, including the rgb dimension) and the **tanh** activation



#### The Discriminator

The discrimanator consists of 3 fully connected layers:

**FC1**: 256 units, LeakyReLU activation with alpha = 0.1, and dropout with rate = 0.2\
**FC2**: 256 units, LeakyReLU activation with alpha = 0.1, and dropout with rate = 0.2\
**FC3 (output layer)**: 1 unit and the **sigmoid** activation

**Optimizer**: Adam with learning rate = 1e-3

#### Hyperparameters
I trained the networks for 30,000 epochs. At each epoch a batch of 32 (batch_size = 32) of real food images are sampled from the food dataset. Another batch of 32 (batch_size = 32) fake images are generated by the generator. 


### Results



# DCGAN

**ref** https://gluon.mxnet.io/chapter14_generative-adversarial-networks/dcgan.html


