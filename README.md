Thanks to FrozenXZeus for helping me with this code and David Sandberg for providing a lot of background code on this.

https://github.com/FrozenXZeus 
https://github.com/davidsandberg

# 1) Install all dependencies:
pip install -r requirements.txt

# 2) Download a pre-trained facenet model.

This part handles the face -> features conversion. I did not include it in the git repository as it is very large.

The quickest way is to download this folder https://drive.google.com/file/d/0B5MzpY9kBtDVZ2RpVDYwWmxoSUk/edit
and put the folder under the facenet_model folder.


You facenet_model folder should like this for the code to work smoothly
- 20170512-110547
    - 20170512-110547.pb
    - model-20170512-110547.ckpt-250000.data-00000-of-00001
    - model-20170512-110547.ckpt-250000.index
    - model-20170512-110547.meta

Other models can be found here https://github.com/davidsandberg/facenet.
If you wish to use a different model, make sure to modify the code as all references are to hte model in the github link.


# 3) Setup the training data (can skip if using provided data)

For every person's face you want to recognize, please make a folder and put images of photos that include their face (and no one else) in there.
A folder with training photos of talk host Graham Norton and rapper 50 Cent would look like
Graham
	- photo 1
	- photo 2
50Cent
    - photo 1
    - photo 2


# 4) Align all the faces and crop the images.

Run the following script, if you want to customise the image size it crops to etc you can do so inside the align_dataset_mtcnn.py file.
See the parse_arguments method for all the different possible commands

src\align\align_dataset_mtcnn.py

Your training_data_aligned folder should now have folders with images inside them. These images have been aligned to make it easier for the facenet network to analyse.
Without this step facenet will not work correctly.

Note 1: If at the very end your classifier is still having issues, try deleting all the files in the /training_data_aligned folder and realign the images
Note 2: This code uses MTCNN to identify faces in an image.

# 5) Train the classifier (SVM Classifier)

Run the following (which is just an easy to use wrapper for classifier.py):
src\classifier_train.py

For optimal performance you will need to tune the batch size depending on how much memory you have on your system.
For a small amount of images any configuration works fine.

To see the different commands for fine tuning see the parse_arguments method in src\classifier.py

# 6) 

## a) Run on a video (saved video or webcam)

run src\newglint.py

First it downloads a video from YouTube into test_data/video, then runs the facenet + the classifier on the video playing at 5x speed.
The video will run slowly because running facenet while playing the video is very computationally intensive, especially on a CPU.

A video recording output.avi will be saved which will playback at a faster speed.

If you want to use the webcam as an input or change the video speed or the video, please see the code under
`if __name__ == "__main__":`

## b) Run on images

Select your test 
# #
Note that these files/folders are in the git ignore
test_data/video/*
training_data_aligned/*
output.avi
facenet_model/*

# #
Note that the images used for training were found public online through Google Image Search and the test video is downloaded from YouTube upon the program running.
I do not claim any ownership to any of these items and am happy to remove them upon request. Any part of the code which may be against the usage terms of any sites or images is not encouraged to be run and is just there for learning reference.