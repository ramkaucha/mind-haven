---
title: Practical Deep Learning
tags:
  - AI
  - Deep-Learning
  - Machine-Learning
cssclasses:
  - pen-white
author:
---
[fast.ai](https://fast.ai)

[Practical Deep Learning Book](https://fastai.github.io/fastbook2e/intro.html)

Its a free course which shows you practical way to apply deep learning and machine learning to practical problems

Software used:
#Pytorch #fastai #transfomers #gradio #python

## What is deep learning ?
Computer technique to extract and transform data, use cases ranging from human speech recognition to animal imagery classification, by using multiple layers of neural networks.

## Course outcomes
- [ ] how to train models that achieve results in
	- [ ] computer vision, including image classification (e.g. pet photos by breed)
	- [ ] neutral language processing (NLP), (e.g. movie review sentiment analysis) and phrase similarity
	- [ ] tabular data with categorical data, continuous data, and mixed ata
	- [ ] collaborative filtering (e.g. move recommendation)
- [ ] how to turn models into web applications and deploy them
- [ ] why and how deep learning models work, and how to use that knowledge to improve the accuracy, speed and reliability of your models
- [ ] the latest deep learning techniques that matter in practice
- [ ] how to implement stochastic gradient descent and a complete training loop from scratch

## Techniques covered:
- random forests and gradient boosting
- affine functions and nonlinearities
- parameters and activations
- transfer learning
- stochastic gradient descent (SGD)
- data augmentation
- weight decay
- image classification
- entity and word embedding

Using Kaggle Jupyter Notebook for model making and tutorials

[Is it a bird? Creating a model from your own data](https://www.kaggle.com/code/ramkaucha/is-it-a-bird-creating-a-model-from-your-own-data/edit)
[Jupyter Notebook 101](https://www.kaggle.com/code/jhoward/jupyter-notebook-101)


```python
# NB: Kaggle requires phone verification to use the internet or a GPU. If you haven't done that yet, the cell below will fail
#    This code is only here to check that your internet is enabled. It doesn't do anything else.
#    Here's a help thread on getting your phone number verified: https://www.kaggle.com/product-feedback/135367

import socket,warnings
try:
    socket.setdefaulttimeout(1)
    socket.socket(socket.AF_INET, socket.SOCK_STREAM).connect(('1.1.1.1', 53))
except socket.error as ex: raise Exception("STOP: No internet. Click '>|' in top right and set 'Internet' switch to on")

# It's a good idea to ensure you're running the latest version of any libraries you need.
# `!pip install -Uqq <libraries>` upgrades to the latest version of <libraries>
# NB: You can safely ignore any warnings or errors pip spits out about running as root or incompatibilities
import os
iskaggle = os.environ.get('KAGGLE_KERNEL_RUN_TYPE', '')

if iskaggle:
    !pip install -Uqq fastai 'duckduckgo_search>=6.2'

from duckduckgo_search import DDGS #DuckDuckGo has changed the api so we need to update 
from fastcore.all import *

def search_images(keywords, max_images=200): return L(DDGS().images(keywords, max_results=max_images)).itemgot('image')
import time, json

#NB: `search_images` depends on duckduckgo.com, which doesn't always return correct responses.
#    If you get a JSON error, just try running it again (it may take a couple of tries).
urls = search_images('dog photos', max_images=1)
urls[0]

from fastdownload import download_url
dest = 'dog.jpg'
download_url(urls[0], dest, show_progress=False)

from fastai.vision.all import *
im = Image.open(dest)
im.to_thumb(256,256)

download_url(search_images('forest photos', max_images=1)[0], 'forest.jpg', show_progress=False)
Image.open('forest.jpg').to_thumb(256,256)

searches = 'forest','dog'
path = Path('dog_or_not')

for o in searches:
    dest = (path/o)
    dest.mkdir(exist_ok=True, parents=True)
    download_images(dest, urls=search_images(f'{o} photo'))
    time.sleep(5)
    resize_images(path/o, max_size=400, dest=path/o)


failed = verify_images(get_image_files(path))
failed.map(Path.unlink)
len(failed)

dls = DataBlock(
    blocks=(ImageBlock, CategoryBlock), 
    get_items=get_image_files, 
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=[Resize(192, method='squish')]
).dataloaders(path, bs=32)

dls.show_batch(max_n=6)

learn = vision_learner(dls, resnet18, metrics=error_rate)
learn.fine_tune(3)

is_bird,_,probs = learn.predict(PILImage.create('dog.jpg'))
print(f"This is a: {is_bird}.")
print(f"Probability it's a bird: {probs[0]:.4f}") 


```

### What is Machine Learning ?
Way to get computers to complete a specific task. Enables computers to learn from data and make decisions or predictions without being explicitly programmed to do so.

![[Pasted image 20241216221700.png]]

![[Pasted image 20241216221706.png]]

### What is Neural Network ?
Flexible function that can be used to solve any given problem, just by varying its weights.
Mathematical proof called the 'universal approximation theorem' shows the possibility, in theory.
![[Pasted image 20241216221738.png]]

