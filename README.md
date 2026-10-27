# ShipSmart - AI Food Item Classifier (START HERE)

You only do TWO things yourself: add your photos, then run a few commands.
Then scan items with your phone using Expo Go.

## What's inside
```
SmartPantry_Complete/
├── dataset/              <- PUT YOUR PHOTOS HERE (one folder per food)
│   ├── apple/  banana/  bread/  egg/
│   └── milk_carton/  tomato/  packaged_snack/  canned_food/
├── split_dataset.py      step 1: splits photos into train/val/test
├── train_model.py        step 2: trains YOUR model
├── app.py + templates/   step 3: server (also a web page) using your model
├── expo_app/             step 4: phone app for Expo Go
├── extract_frames.py     (optional) turns a video into photos
└── requirements.txt
```

## Step 0 - Install (once)
Python 3.10 or 3.11, then in a terminal in this folder:
```
pip install -r requirements.txt
```
The app also loads an open-vocabulary CLIP model the first time it starts.
This model can recognize common foods that are not in your training folders,
including onions and potatoes. An internet connection is needed on first start
to download its weights. Its candidate labels cover common grocery items, but
predictions for unfamiliar foods are not guaranteed.

## Step 1 - Add your photos
Drag photos into the matching folders in `dataset/`.
- 100+ per folder is ideal (30 is enough for a test); vary angle, light, background
- You never paste images into code - the scripts read the folders
- Different foods? Rename/add/delete folders. Folder names become labels.
  Keep at least 2 folders with photos and delete any empty ones.
- Shortcut: film a 30 sec video, then `python extract_frames.py video.mp4 apple`

## Step 2 - Train
```
python split_dataset.py
python train_model.py
```
Accuracy numbers for your report: `model/results_summary.txt`, `training_history.png`.

## Step 3 - Start the server
```
python app.py
```
It prints the address to use on your phone. (You can also open
http://127.0.0.1:5000 on the computer. Green banner = your own model.)

## Step 4 - Use your phone with Expo Go
Follow `expo_app/README.md`.

## Host a private-code website for phone browsers

The Flask server and model must run on a host; the HTML page by itself cannot
classify photos. To give phone users a secure HTTPS link without publishing the
project source, deploy this folder from a **private GitHub repository** to
Render using the included `Dockerfile` and `render.yaml`. The Render web service
is public, but its source repository remains private. The hosting provider
receives the application and model files; people using the website do not get
access to your computer's files.

1. Put this project folder's contents in a private GitHub repository, including
   `model/food_classifier.h5`, `model/class_names.json`, `Dockerfile`, and
   `render.yaml`. Do not upload `.venv/` or `dataset/`.
2. In Render, create a Blueprint from that private repository and deploy the
   `shipsmart` web service. The included configuration uses a paid Standard
   instance because the running app uses about 1.1 GB of memory on this
   computer; free and Starter plans may run out of memory.
3. Wait for the first deployment to finish downloading the CLIP model. Render's
   URL uses HTTPS; open it in Chrome on your phone. The HTTPS site can also ask
   for camera permission.
4. Keep the GitHub repository private. Anyone with the Render URL can use the
   classifier, but they cannot browse the source through the website.

## Troubleshooting
- "No images found yet": put photos in the `dataset/` folders first.
- Training errors about empty classes: delete empty folders in `dataset/`.
- Training slow: use Google Colab or fewer images first.
- After retraining, restart `app.py`.
