# Mirror
## Pytorch CNN Visualisation Tool

This is a raw beta so expect lots of things to change and improve over time.

![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/master/resources/mirror.gif?raw=true)

### Getting started

To install mirror run

```
pip install git+https://github.com/FrancescoSaverioZuppichini/mirror.git
```

Basic example:

```python
from mirror import mirror
from mirror.visualisations import *

from PIL import Image

from torchvision.models import resnet101, resnet18, vgg16, alexnet
from torchvision.transforms import ToTensor, Resize, Compose

# create a model
model = vgg16(pretrained=True)
# get an image
cat = Image.open("./cat.jpg")
# resize the image and make it a tensor
input = Compose([Resize((224,224)), ToTensor()])(cat)
# add 1 dim for batch
input = input.unsqueeze(0)
# call mirror with the input and the model
mirror(input, model, visualisations=[DeepDreamVis, BackPropVis, GradCamVis])
```

It will automatic open a new tab in your browser

![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/develop/resources/mirror.jpg?raw=true)

### Available Visualisations
#### Weights
![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/develop/resources/weights.png?raw=true)
### Deep Dream
![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/develop/resources/deepdream.png?raw=true)
#### Back Prop / Guide Back Prop
By clicking on the radio button 'guide', all the relus negative output will be set to zero producing a nicer looking image
![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/develop/resources/backprop.png?raw=true)
### Grad Cam / Guide Grad Cam
- [ ] Add text field for class
- [ ] 
![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/develop/resources/grad_cam.png?raw=true)
### Create a Visualisation

You can find an example below

```python
from mirror.visualisations.Visualisation import Visualisation

class DummyVisualisation(Visualisation):

    def __call__(self, inputs, layer):
        return inputs.repeat(self.params['repeat']['value'],1, 1, 1)

    @property
    def name(self):
        return 'dummy'

    def init_params(self):
        return {'repeat' : {
                 'type' : 'slider',
                 'min' : 1,
                 'max' : 100,
                 'value' : 3,
                 'step': 1,
                 'params': {}
                 }}

```

![alt](https://github.com/FrancescoSaverioZuppichini/mirror/blob/develop/resources/dummy.jpg?raw=true)

The `__call__` function is called each time you click a layer or change a value in the options on the right.

The `init_params`  function returns a dictionary of options that will be showed on the right drawer of the application. For now only `slider` and `radio` are supported

### TODO
- [x] Cache reused layer 
- [x] Make a generic abstraction of a visualisation in order to add more features  
- [x] Add dropdown as parameter
- [ ] Add text field
- [ ] Support multiple inputs
- [ ] Support multiple models
- Add all visualisation present here https://github.com/utkuozbulak/pytorch-cnn-visualizations
    * [x] [Gradient visualization with vanilla backpropagation](#gradient-visualization)
    * [x] [Gradient visualization with guided backpropagation](#gradient-visualization) [1]
    * [ ] [Gradient visualization with saliency maps](#gradient-visualization) [4]
    * [ ] [Gradient-weighted [3] class activation mapping](#gradient-visualization) [2] 
    * [ ] [Guided, gradient-weighted class activation mapping](#gradient-visualization) [3]
    * [ ] [Smooth grad](#smooth-grad) [8]
    * [x] [CNN filter visualization](#convolutional-neural-network-filter-visualization) [9]
    * [ ] [Inverted image representations](#inverted-image-representations) [5]
    * [x] [Deep dream](#deep-dream) [10]
    * [ ] [Class specific image generation](#class-specific-image-generation) [4]
    * [x] [Grad Cam](https://arxiv.org/abs/1610.02391)
