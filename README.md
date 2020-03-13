# deep cross modal hashing (torchcmh)

torchcmh is a library built on PyTorch for deep learning cross modal hashing.\
Including: 
- data visualization
- baseline methods
- multiple data reading API
- loss function API
- config call
----
There are four datasets(Mirflickr25k, Nus Wide, MS coco, IAPR TC-12) sort out by myself,
if you want use these datasets, please download mat file and image file by readme file in dataset package.

you need to install these package to run
- visdom 0.1.8+
- pytorch 1.0.0+
- tqdm 4.0+
----
### how to using
- create a configuration file as ./script/default_config.yml
```yaml
training:
  # the name of python file in training
  method: SCAHN
  # the data set name, you can choose mirflickr25k, nus wide, ms coco, iapr tc-12
  dataName: Mirflickr25K
  batchSize: 64
  # the bit of hash codes
  bit: 64
  # if true, the program will be run on gpu. Of course, you need install 'cuda' and 'cudnn' better.
  cuda: True
  # the device id you want to use, if you want to multi gpu, you can use [id1, id2]
  device: 0
datasetPath:
  Mirflickr25k:
    # the path you download the image of data set. Attention: image files, not mat file.
    img_dir: \dataset\mirflickr25k\mirflickr

```
- run ./script/main.py and input configuration file path.
```python
from torchcmh.run import run
if __name__ == '__main__':
    run(config_path='default_config.yml')
```
----
### how to create your method
- create new method file in folder ./torchcmh/training/
- inherit implement TrainBase
- change the training.method as your python file name in config .yml file and run.

#### some function in TrainBase
- data visualization
```python
def plot_loss(self, title: str, loss_store=None):   # args:title is the title of graph
    if loss_store is None:
        loss_store = self.loss_store
    if self.plotter:
        for name, loss in loss_store.items():
            self.plotter.plot(title, name, loss.avg)
```
- valid
```python
for epoch in range(self.max_epoch):
    # training codes
    self.valid(epoch)
```
----
### LICENSE
this repository keep MIT license.