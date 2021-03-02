# face-mask-detection  
![Picture1](https://user-images.githubusercontent.com/49630112/109633774-15b04700-7b7b-11eb-844c-13ebda1457f1.png)  
Face mask detection project can be used to detect people wearing mask, not wearing mask and wearing mask incorrectly, then track them by realtime. The model was built based on tiny-yolov4 model [here](https://github.com/AlexeyAB/darknet) and MOSSE object tracker. this system achieves 90% mAP and 3ms mean inference time.

## Dataset
This project uses kaggle ***face mask detection*** dataset
This dataset contains 853 images belonging to the 3 classes, as well as their bounding boxes in the PASCAL VOC format.
The classes are:  
- With mask;
- Without mask;
- Mask weared incorrect.   
    
You can download this dataset [here](https://www.kaggle.com/andrewmvd/face-mask-detection)  
Because this dataset is imbalance, we proceeded to collect more data belongs to class **With mask** and **Mask weared incorrect**, combined with data augmentation to resolve the issue. The final dataset can be describle as below:
- Total number of images: 6064  
- With mask: 6806  
- Without mask: 7636  
- Mask weared incorrect: 4572  

## Object detection using tiny yolo v4  
This project was implemented on google colaboratory by steps below:  
- Make a copy of this google drive folder: [here](https://drive.google.com/drive/folders/1Aaw6nMELhfQa67WIaPPX3K_ywmAggoJA?usp=sharing)  
- Extract train.rar and test.rar <code>*!unrar x test.rar /content/*</code>    
- Change directory to **darknet** folder  
- Compile Darknet with GPU=1 CUDNN=1 CUDNN_HALF=1 OPENCV=1 in the Makefile  
- Start training: <code>*! ./darknet detector train data/obj.data yolov4-obj.cfg yolov4.conv.137 -dont_show -map*</code>  
    
## How to infer the model:  
After training phase, weight files will be saved in ./backup/ folder. User the yolov4-tiny-obj_best.weight to infer the images in test set:  
<code>*!python darknet_images.py --input '/content/test' --weight ./backup/yolov4-tiny-obj_best.weights --save_labels --config_file yolov4-tiny-obj.cfg --data_file obj.data*</code>  
Labels coordinate with images in test set with be saved in the same directory with the test image as text file with the filename is the image name (replace .jpg by .txt). Each line of text file can be describled as below:  
<code>*object-class x_center y_center width height*</code>  
 The object-class was one-hot encoded:  
  - 0: Without mask  
  - 1: With mask  
  - 2: Mask weared incorrect  

## Object tracking based upon MOSSE  

