# Open nsfw model
This repo contains code for running Not Suitable for Work (NSFW) classification deep neural network Caffe models.
#### Not suitable for work classifier
Detecting offensive / adult images is an important problem which researchers have tackled for decades. With the evolution of computer vision and deep learning the algorithms have matured and we are now able to classify an image as not suitable for work with greater precision.

Defining NSFW material is subjective and the task of identifying these images is non-trivial. Moreover, what may be objectionable in one context can be suitable in another. For this reason, the model we describe below focuses only on one type of NSFW content: pornographic images. The identification of NSFW sketches, cartoons, text, images of graphic violence, or other types of unsuitable content is not addressed with this model.

Since images and user generated content dominate the internet today, filtering nudity and other not suitable for work images becomes an important problem. In this repository we opensource a Caffe deep neural network for preliminary filtering of NSFW images. 

#### Usage

* The network takes in an image and gives output a probability (score between 0-1) which can be used to filter not suitable for work images. Scores < 0.2 indicate that the image is likely to be safe with high probability. Scores > 0.8 indicate that the image is highly probable to be NSFW. Scores in middle range may be binned for different NSFW levels. 
* Depending on the dataset, usecase and types of images, we advise developers to choose suitable thresholds. Due to difficult nature of problem, there will be errors, which depend on use-cases / definition / tolerance of NSFW.  Ideally developers should create an evaluation set according to the definition of what is safe for their application, then fit a [ROC](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) curve to choose a suitable threshold if they are using the model as it is. 
* ***Results can be improved by [fine-tuning](http://caffe.berkeleyvision.org/gathered/examples/finetune_flickr_style.html)*** the model for your dataset/ uscase / definition of NSFW. We do not provide any guarantees of accuracy of results. Please read the disclaimer below.
* Using human moderation for edge cases in combination with the machine learned solution will help improve performance.

#### Docker Quickstart

Build a caffe docker image (CPU) 
```
docker build -t caffe:cpu https://raw.githubusercontent.com/BVLC/caffe/master/docker/standalone/cpu/Dockerfile
```

Check the caffe installation
```
docker run caffe:cpu caffe --version
caffe version 1.0.0-rc3
```

Run the docker image with a volume mapped to your `open_nsfw` repository. Your `test_image.jpg` should be located in this same directory.
```
cd open_nsfw
docker run --volume=$(pwd):/workspace caffe:cpu \
python ./classify_nsfw.py \
--model_def nsfw_model/deploy.prototxt \
--pretrained_model nsfw_model/resnet_50_1by2_nsfw.caffemodel \
test_image.jpg
```

