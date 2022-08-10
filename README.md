# Attention (Installation)

You should follow instructions in this [repo](https://github.com/simurgailab/installation-guide-of-maskrcnn) for proper installation.

# Mask R-CNN Implementation With Custom Dataset

## Dataset Configuration

After importing libraries, you have to specify class count as "Background + Classcount" in configuration step:

```
    # Number of classes (including background)
    NUM_CLASSES = 1 + 5  # Background + classname, for instance, if there are 5 classes, specify 1+5.
``` 

In creating a synthetic dataset step custom classes has to been added to self attribute:
```
class customDataset(utils.Dataset):

    def load_class(self, dataset_dir, subset):
    ...
        self.add_class("configclassname", 1, "class1")
        self.add_class("configclassname", 2, "class2")
        self.add_class("configclassname", 3, "class3")
        self.add_class("configclassname", 4, "class4")
        self.add_class("configclassname", 5, "class5")
    ...
```

After previous step,  for matching polygons and images.
```
class customDataset(utils.Dataset):

    def load_class(self, dataset_dir, subset):
    ...
        self.add_class("configclassname", 1, "class1")
        self.add_class("configclassname", 2, "class2")
        self.add_class("configclassname", 3, "class3")
        self.add_class("configclassname", 4, "class4")
        self.add_class("configclassname", 5, "class5")
    ...
```