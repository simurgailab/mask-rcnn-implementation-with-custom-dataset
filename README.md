# Attention (Installation)

You should follow instructions in this [repo](https://github.com/simurgailab/installation-guide-of-maskrcnn) for proper installation.

# Mask R-CNN Implementation With Custom Dataset

## Dataset Configuration

After importing libraries, class count as "Background + Classcount" has to been specify in configuration step:

```
    # Number of classes (including background)
    NUM_CLASSES = 1 + 5  # Background + classname, for instance, if there are 5 classes, specify 1+5.
``` 

Custom classes has to been added to self attribute in creating a synthetic dataset step:
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

After previous step, annotation file (.json) has to been defined as custom. 
Meanwhile, attention has to be on the index value in this step. The index values are related to your custom json file.
```
class customDataset(utils.Dataset):

    def load_class(self, dataset_dir, subset):
    ...
        annotations1 = json.load(open(os.path.join(dataset_dir, "dataset.json")))
        
        # 2nd Key in Json File: Annotation key elements in json file
        annotations = list(annotations1.values())[2]  # don't need the dict keys

        # 1st Key in Json File: Images key elements in json file
        images= list(annotations1.values())[1]  # don't need the dict keys
    ...
```
Load_mask function has been defined for matching polygons and images that have been given its infos.
```
class customDataset(utils.Dataset):
    ...
    def load_mask(self, image_id):
    ...
```

