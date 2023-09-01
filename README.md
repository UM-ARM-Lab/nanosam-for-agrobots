# NanoSAM

NanoSAM is a Segment Anything (SAM) model variant that is capable of running in real-time on NVIDIA Jetson platforms with NVIDIA TensorRT.  


*Why NanoSAM?*

While other lightweight SAM architectures, like MobileSAM, already exist, we find that after TensorRT optimization, these models are still bottlenecked by the image encoder and achieve sub-realtime framerates.  NanoSAM is trained by distilling the MobileSAM image
encoder into an architecture that runs an order of magnitude faster on NVIDIA Jetson with little
loss in accuracy. This enables real-time inference and unlocks new applications like converting bounding box detections into instance segmentations and performing segmentation based tracking.

## Benchmarks

<table style="border-top: solid 1px; border-left: solid 1px; border-right: solid 1px; border-bottom: solid 1px">
    <thead>
        <tr>
            <th rowspan=2 style="text-align: center; border-right: solid 1px">Model</th>
            <th colspan=2 style="text-align: center; border-right: solid 1px">Jetson Orin Nano (ms)</th>
            <th colspan=2 style="text-align: center; border-right: solid 1px">Jetson AGX Orin (ms)</th>
            <th colspan=3 style="text-align: center; border-right: solid 1px">Accuracy (mIoU)</th>
            <th rowspan=2 style="text-align: center; border-right: solid 1px">Download</th>
        </tr>
        <tr>
            <th style="text-align: center; border-right: solid 1px">Image Encoder</th>
            <th style="text-align: center; border-right: solid 1px">Full Pipeline</th>
            <th style="text-align: center; border-right: solid 1px">Image Encoder</th>
            <th style="text-align: center; border-right: solid 1px">Full Pipeline</th>
            <th style="text-align: center; border-right: solid 1px">Small</th>
            <th style="text-align: center; border-right: solid 1px">Medium</th>
            <th style="text-align: center; border-right: solid 1px">Large</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align: center; border-right: solid 1px">MobileSAM</td>
            <td style="text-align: center; border-right: solid 1px"></td>
            <td style="text-align: center; border-right: solid 1px">146</td>
            <td style="text-align: center; border-right: solid 1px">35</td>
            <td style="text-align: center; border-right: solid 1px">39</td>
            <td style="text-align: center; border-right: solid 1px">0.728</td>
            <td style="text-align: center; border-right: solid 1px">0.759</td>
            <td style="text-align: center; border-right: solid 1px">0.804</td>
            <td style="text-align: center; border-right: solid 1px"><a href="#">ONNX</a></td>
        </tr>
        <tr>
            <td style="text-align: center; border-right: solid 1px">NanoSAM (ResNet18)</td>
            <td style="text-align: center; border-right: solid 1px"></td>
            <td style="text-align: center; border-right: solid 1px">27</td>
            <td style="text-align: center; border-right: solid 1px">4.2</td>
            <td style="text-align: center; border-right: solid 1px">8.1</td>
            <td style="text-align: center; border-right: solid 1px">0.704</td>
            <td style="text-align: center; border-right: solid 1px">0.738</td>
            <td style="text-align: center; border-right: solid 1px">0.796</td>
            <td style="text-align: center; border-right: solid 1px"><a href="#">ONNX</a></td>
        </tr>
    </tbody>
</table>

## Examples

### Detection to segmentation upgrading

### Segmentation based tracking


## Usage

### Create predictor

```python3
from nanosam import Predictor

predictor = Predictor(
    image_encoder="nanosam_resnet18.engine", 
    mask_decoder="sam_mask_decoder.engine"
)
```

### Segment with points

```python3
import PIL.Image 

image = PIL.Image.open("dog.jpg")

points = np.array([
    [x0, y0],  # foreground point
    [x1, y1],  # foreground point
    [x2, y2]   # background point
])

point_labels = np.array([
    1,  # foreground point
    1,  # foreground point
    0   # background point
])

predictor.set_image(image)

mask, iou, mask_small = predictor.predict(points, point_labels)
```

### Segment with bounding box

```python3
points = np.array([
    [x0, y0],   # bbox top-left
    [x1, y1]    # bbox bottom-right
])

point_labels = np.array([
    2,  # bbox top-left
    3   # bbox bottom-right
])

mask, iou, mask_small = predictor.predict(points, point_labels)
```


## Training


## Evaluation

## Acknowledgement

- [SAM](#)
- [MobileSAM](#) 