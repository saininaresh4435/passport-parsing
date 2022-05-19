A passport parsing service based on ROI and Tesseract:
input : image url/ local image path
ouput : [roi]
# ROI:  Passport number, Name, surname, date of issue, date of birth, date of expiry etc.

##### To install packages run this command line
```bash
$ pip install -r requirements.txt 
```

#### Config Parameters
```bash
optional arguments:
  --SUPER_RESOLUTION       Image super resolution to enhance ocr accurracy(True/False).
  --DRAW_BOX               To draw ROI bounding box for each class(True/False)
  --SAVE_PATH_BOX          Path to save bounding box images 
  --CROP_SAVE              To crop ROI just to check if cropping is good or not for OCR(True/False)
  --CROP_SAVE_PATH         Path to save cropped ROI
  --LOCAL_IMG_PATH         Local image path to extract ROI 
```

#### Model 
All the model downloading are doing in start.sh module.

1. ROI detection model(uploaded on s3 bucket)
2. Image super resolution model(uploaded on s3 bucket)
3. eng.traineddata for OCR(Download during starting of the service)
4. Latin.traineddata for OCR(Download during starting of the service)

#### Url And Request body

postman-collection folder includes postman urls and request body. We have 2 request body one for testing service through image url and another one from local image path.


### How to run

```bash
python app.py
```


#### Docker testing

Build Image:

```bash
docker build -t passport-parsing .
```

Docker Run:

```bash
docker run -d -p 5050:5000 image-id
```

Docker Run with volume mount:

```bash
docker run -d -v ./input:/app/input  -p 5050:5000 image-id
```
#### Train ROI detection model

Use this repo to setup ROI detection model and training on custom dataset:

https://github.com/project-anuvaad/ocr-toolkit/tree/ocr_toolkit/layout-model-training


#### Sample Output:
```bash
{
    "config": {
        "language": {
            "sourceLanguage": "en"
        },
        "superresolution": "False"
    },
    "output": [
        {
            "source": {
                "Date of Birth": "01/11/1973",
                "Date of Expiry": "01/05/2016",
                "Date of Issue": "02/05/2006",
                "First Name": "AISHWARYA",
                "Gender": "F",
                "Nationality": "INDIAN",
                "Place of Birth": "MANGALORE KARNATAKA",
                "Place of Issue": "MUMBAI",
                "Surname": "RAI",
                "passport no.": "F7802033"
            }
        }
    ]
}
```