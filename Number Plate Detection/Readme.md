Dataset of number plates is downloaded from OIDv4 website.we can do it in command line as below

    Python3 is required.

    Clone this repository
    
      git clone https://github.com/EscVM/OIDv4_ToolKit.git
    
    Install the required packages
      
      pip install -r requirements.txt
      
      python main.py downloader --classes Apple  --type_csv validation
      
        replace Apple with custom class in this case number_plate
        
    open images has specific format of annotations like 

        nameoftheclass left top right bottom

    but we have to convert those into our formats like yolo,tf format like 
    
        classid,xmin,ymin,xmax,ymax


    to do this type of conversion to yolo we have to run convert_annotations.py from OID folder.

      1) first we need conver_annotations.py file (uploaded here) to convert and classes.txt file which has line separated classes names.

      2) place this two files in OID folder 


Train yolo model and save weights file in yolov4-custom-functions\data folder.

Open anaconda prompt and Goto yolo custom functions folder in my case  C:\Users\SURYA\Documents\number_plate\yolov4-custom-functions>

Run below commands 
    
      conda create -n license_plate_detection python=3.6
      
      conda activate license_plate_detection
      
      pip install -r requirements.txt
      
      
convert yolo .weights file to tensorflow protobuf file .pb using below command  - 

    python save_model.py --weights ./data/yolov4-obj_best.weights --output ./checkpoints/custom-416 --input_size 416 --model yolov4
    
      which saves .pb file in ./checkpoints/custom-416 folder
      
use this protobuf file for custom predictions using below command - 

    python detect.py --weights ./checkpoints/custom-416 --size 416 --model yolov4 --images ./data/images/car.jpg --plate
    
        if we want to predict custom image place the image in ./data/images/ folder.Replace car.jpg with custom image.
