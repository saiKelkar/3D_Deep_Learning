(https://www.mathworks.com/help/lidar/ref/randlanet.html)

RandLA-Net 
- designed for semantic segmentation for large scale point clouds
- uses random sampling to down sample large point clouds and boost speed
- employs local feature aggregation module to preserve significant features

- create a pretrained or untrained semantic segmentation network using the randlanet object
- if pretrained, perform transfer learning and retrain it using the trainRandlanet function
- if untrained, train it using trainRandlanet function
- segment objects in a point cloud using the segmentObjects function on pretrained, retrained, or newly trained segmentation network

```
Pretrained RandLA-Net 
segmenter = randlanet(weights) creates a pretrained network with the weights determined by the data set on which the network is pretrained. 

Custom RandLA-Net
segmenter = randlanet(weights, classNames) creates a network, and configures it to perform segmentation using the specified set of classes
```

(https://github.com/QingyongHu/RandLA-Net)

(https://www.open3d.org/docs/latest/python_api/open3d.ml.torch.models.RandLANet.html)

(https://developers.arcgis.com/python/latest/guide/point-cloud-classification-using-randlanet/)