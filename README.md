# Computer Pointer Controller

Computer Pointer Controller is an application using the gaze detection model to control the mouse pointer of the computer, you can read more from the link to doc. [Gaze Detection Model](https://docs.openvinotoolkit.org/latest/_models_intel_gaze_estimation_adas_0002_description_gaze_estimation_adas_0002.html).

## Project Set Up and Installation
1. Download **[OpenVino Toolkit 2020.1](https://docs.openvinotoolkit.org/latest/index.html) follow the guide for easy installations.
2. Clone the Repository from :https://github.com/adesojisusan/Computer-Pointer-Controller.
3.Download model from "OpenVino Zoo" using below 4(four) commands.
```
python3 /opt/intel/openvino/deployment_tools/open_model_zoo/tools/downloader/downloader.py --name gaze-estimation-adas-0002 
python3 /opt/intel/openvino/deployment_tools/open_model_zoo/tools/downloader/downloader.py --name face-detection-adas-binary-0001  
python3 /opt/intel/openvino/deployment_tools/open_model_zoo/tools/downloader/downloader.py --name head-pose-estimation-adas-0001  
python3 /opt/intel/openvino/deployment_tools/open_model_zoo/tools/downloader/downloader.py --name landmarks-regression-retail-0009
```

## Demo
(https://www.youtube.com/watch?v=mg-aCtADj0)

 
## Benchmarks
I checked Model Loading Time,Frames Per Second model, Inference Time,for `FP16`, `FP32`, and `FP32-INT8` of all required models.

`FP16`: 
```
python main.py -fd ../intel/face-detection-adas-binary-0001/FP32-INT1/face-detection-adas-binary-0001.xml \ 
-lr ../intel/landmarks-regression-retail-0009/FP16/landmarks-regression-retail-0009.xml \ 
-hp ../intel/head-pose-estimation-adas-0001/FP16/head-pose-estimation-adas-0001.xml \ 
-ge ../intel/gaze-estimation-adas-0002/FP16/gaze-estimation-adas-0002.xml \ 
-d CPU -i ../bin/demo.mp4 -o results/FP16/ -flags ff fl fh fg
```

`FP32`: 
```
python main.py -fd ../intel/face-detection-adas-binary-0001/FP32-INT1/face-detection-adas-binary-0001.xml \ 
-lr ../intel/landmarks-regression-retail-0009/FP32/landmarks-regression-retail-0009.xml \ 
-hp ../intel/head-pose-estimation-adas-0001/FP32/head-pose-estimation-adas-0001.xml \ 
-ge ../intel/gaze-estimation-adas-0002/FP32/gaze-estimation-adas-0002.xml \ 
-d CPU -i ../bin/demo.mp4 -o results/FP32/ -flags ff fl fh fg
```

`FP32-INT8`: 
```
python main.py -fd ../intel/face-detection-adas-binary-0001/FP32-INT1/face-detection-adas-binary-0001.xml \ 
-lr ../intel/landmarks-regression-retail-0009/FP32-INT8/landmarks-regression-retail-0009.xml \ 
-hp ../intel/head-pose-estimation-adas-0001/FP32-INT8/head-pose-estimation-adas-0001.xml \ 
-ge ../intel/gaze-estimation-adas-0002/FP32-INT8/gaze-estimation-adas-0002.xml \ 
-d CPU -i ../bin/demo.mp4 -o results/FP32-INT8/ -flags ff fl fh fg
```


## Results
'FP16' has lowest model time and `FP32-INT8` has highest model loading time.
'Inference Time' and `FPS`, `FP32` give a good resul.
'Asynchronous Inference has better results than Synchronous Inference,it has slight upgrade in `inference time` and `FPS`



**Synchronous Inference**
precisions = ['FP16', 'FP32', 'FP32-INT8']
Inference Time : [26.6, 26.4, 26.9]
fps : [2.218045112781955, 2.234848484848485, 2.193308550185874]
Model Load Time : [1.6771371364593506, 1.6517729759216309, 5.205628395080566]

### Async Inference
precisions = ['FP16', 'FP32', 'FP32-INT8']
Inference Time : [23.9, 24.7, 24.0]
fps : [2.468619246861925, 2.388663967611336, 2.4583333333333335]
Model Load Time : [0.7770581245422363, 0.7230548858642578, 2.766681432723999

### Edge Cases
No Head Detection: it will skip the frame and inform the user
