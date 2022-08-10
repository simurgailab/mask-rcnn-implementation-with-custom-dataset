# Attention (Installation)

You should follow instructions in this [repo](https://github.com/simurgailab/installation-guide-of-maskrcnn) for proper installation.

# Mask R-CNN Object Detection Architecture

Faster R-CNN has two outputs for each candidate object: a class label and a bounding box offset. In Mask R-CNN, in addition to these outputs, a branch that extracts the object mask is added. However, this mask output is quite different from the class and box output. In addition, a difference from Fast R-CNN and Faster R-CNN is that the pixel-to-pixel alignment method is used in Mask R-CNN.

While examining Mask R-CNN, we should remember that Faster R-CNN consists of two stages. The first stage, called the District Proposal Network, proposes candidate bounding boxes. The second phase, which is actually Fast R-CNN, does RoI Pooling (Region of Interest Pooling) from each candidate tile. In this way, it performs classification and bounding box regression by extracting features. To put it briefly, Mask R-CNN adopts this two-step procedure that Faster R-CNN has. Mask R-CNN outputs a binary mask for each RoI in parallel with the class and tile address outputs in the second stage.

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

