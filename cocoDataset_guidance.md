### COCODataset

data/datasets/coco.py define a class, COCODataset that inherits from torchvision.datasets.coco.CocoDetection.
torchvision.datasets.coco.CocoDetection is shown as following:
```
import torch.utils.data as data
from PIL import Image
import os
import os.path

class CocoDetection(data.Dataset):
    """`MS Coco Detection <http://mscoco.org/dataset/#detections-challenge2016>`_ Dataset.
    Args:
        root (string): Root directory where images are downloaded to.
        annFile (string): Path to json annotation file.
        transform (callable, optional): A function/transform that  takes in an PIL image
            and returns a transformed version. E.g, ``transforms.ToTensor``
        target_transform (callable, optional): A function/transform that takes in the
            target and transforms it.
    """

    def __init__(self, root, annFile, transform=None, target_transform=None):
        from pycocotools.coco import COCO
        self.root = root
        self.coco = COCO(annFile)
        self.ids = list(self.coco.imgs.keys())
        self.transform = transform
        self.target_transform = target_transform

    def __getitem__(self, index):
        """
        Args:
            index (int): Index
        Returns:
            tuple: Tuple (image, target). target is the object returned by ``coco.loadAnns``.
        """
        coco = self.coco
        img_id = self.ids[index]
        ann_ids = coco.getAnnIds(imgIds=img_id)
        target = coco.loadAnns(ann_ids)

        path = coco.loadImgs(img_id)[0]['file_name']
        img = Image.open(os.path.join(self.root, path)).convert('RGB')
        
        if self.transform is not None:
            img = self.transform(img)

        if self.target_transform is not None:
            target = self.target_transform(target)

        return img, target

    def __len__(self):
        return len(self.ids)

    ...
```

CocoDetection init a pycocotools.coo instance inside.
```
from pycocotools.coco import COCO
self.coco = COCO(annFile)
```
maskrcnn's COCODataset will call self.coco to get some variable about coco dataset.
pycocotools.coco is shown in: https://github.com/cocodataset/cocoapi/blob/master/PythonAPI/pycocotools/coco.py
