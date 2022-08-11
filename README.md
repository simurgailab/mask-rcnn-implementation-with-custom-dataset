# Attention (Installation)

You should follow instructions in this [repo](https://github.com/simurgailab/installation-guide-of-maskrcnn) for proper installation.

# Mask R-CNN Object Detection Architecture

Faster R-CNN has two outputs for each candidate object: a class label and a bounding box offset. In Mask R-CNN, in addition to these outputs, a branch that extracts the object mask is added. However, this mask output is quite different from the class and box output. In addition, a difference from Fast R-CNN and Faster R-CNN is that the pixel-to-pixel alignment method is used in Mask R-CNN.

While examining Mask R-CNN, we should remember that Faster R-CNN consists of two stages. The first stage, called the District Proposal Network, proposes candidate bounding boxes. The second phase, which is actually Fast R-CNN, does RoI Pooling (Region of Interest Pooling) from each candidate tile. In this way, it performs classification and bounding box regression by extracting features. To put it briefly, Mask R-CNN adopts this two-step procedure that Faster R-CNN has. Mask R-CNN outputs a binary mask for each RoI in parallel with the class and tile address outputs in the second stage.


# Mask R-CNN Implementation With Custom Dataset

## Dataset Configuration

&rarr; After importing libraries, class count as "Background + Classcount" has to been specify in configuration step:

```
    # Number of classes (including background)
    NUM_CLASSES = 1 + 5  # Background + classname, for instance, if there are 5 classes, specify 1+5.
``` 

&rarr; Custom classes has to been added to self attribute in creating a synthetic dataset step. In here "configclassname" specifies the superclass in json annotation file inside the "Categories" key.
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

&rarr; After previous step, annotation file (.json) has to been defined as custom. 
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
&rarr; Load_mask function has been defined for matching polygons and images that have been given its infos.
```
class customDataset(utils.Dataset):
    ...
    def load_mask(self, image_id):
    ...
```
&rarr; "configclassname" also has to be changed in following sections.
```
    def image_reference(self, image_id):
        ...
        if info["source"] == "configclassname":
            ...
```
```
class customDataset(utils.Dataset):

    def load_class(self, dataset_dir, subset):
        for img in images:
            ...
            self.add_image(
                "configclassname", #custom super class name
                ...
```

```
class customDataset(utils.Dataset):
    def load_mask(self, image_id):
        if image_info["source"] != "configclassname":
            ...
```

# Appendices

More info about Mask R-CNN:

&rarr; **Medium**
* [Mask R-CNN Overlapping Bounding Boxes Problem](https://pub.towardsai.net/mask-r-cnn-overlapping-bounding-boxes-problem-a9582d41875b)
* [Nesne Tespit Algoritmalarının Gelişimi (CNN, R-CNN, Fast R-CNN ve Faster R-CNN) — 1](https://dilaraozdemir.medium.com/nesne-tespit-algoritmalar%C4%B1n%C4%B1n-geli%C5%9Fimi-cnn-r-cnn-fast-r-cnn-ve-faster-r-cnn-1-521ab97071a0)
* [Nesne Tespit Algoritmalarının Gelişimi (Mask R-CNN) — 2](https://dilaraozdemir.medium.com/nesne-tespit-algoritmalar%C4%B1n%C4%B1n-geli%C5%9Fimi-mask-r-cnn-2-b622f6f4c2a8)
* [R — CNN Ailesi Part II: Faster R-CNN & Mask R-CNN](https://elifmeseci.medium.com/r-cnn-ailesi-part-ii-76cce9e4a9d6)
* [R-CNN Ailesi Part I: R-CNN & Fast R-CNN](https://elifmeseci.medium.com/r-cnn-ailesi-part-i-6aaf775d05d2)


&rarr; **Videos**
* [Adım Adım Sıfırdan Mask R-CNN Projesi Oluşturma](https://youtu.be/9ZNtavU1asE)
* [Adım Adım Özel Veri Kümesinde Mask R-CNN Uygulaması](https://youtu.be/Ldnmxa4pM3g)
* [Zero to Hero: Mask RCNN Project](https://youtu.be/cpSa0WMAkzY)


# References

* He, Kaiming, et al. ["Mask r-cnn."](https://arxiv.org/abs/1703.06870) Proceedings of the IEEE international conference on computer vision. 2017.

* Girshick, Ross. ["Fast r-cnn."](https://openaccess.thecvf.com/content_iccv_2015/papers/Girshick_Fast_R-CNN_ICCV_2015_paper.pdf) Proceedings of the IEEE international conference on computer vision. 2015.

* Ren, Shaoqing, et al. ["Faster r-cnn: Towards real-time object detection with region proposal networks."](https://proceedings.neurips.cc/paper/2015/file/14bfa6bb14875e45bba028a21ed38046-Paper.pdf) Advances in neural information processing systems 28 (2015).
* Reference Repo: [Mask R-CNN for Object Detection and Segmentation](https://github.com/matterport/Mask_RCNN/)
* Original repo is available at [Detectron](https://github.com/facebookresearch/Detectron)
* Lin, Tsung-Yi, et al. ["Microsoft coco: Common objects in context."](https://arxiv.org/abs/1405.0312) European conference on computer vision. Springer, Cham, 2014.