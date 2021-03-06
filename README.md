# face-recognition


Face detection technology has widely attracted attention due to its enormous application value and market potential, such as face recognition and video surveillance system. Real-time face detection not only is one part of the automatic face recognition system but also is developing an independent research subject. So, there are many approaches to solve face detection. An application for tracking and detecting faces in videos and in cameras which can be used for multipurpose activities. The intention of the paper is deep study of face detection using openCV. A tabular comparison is performed in order to understand the algorithms in an easier manner. It talks about various algorithms like cascades classifier. This paper aims to help in understanding the best prerequisites for face recognition.


Face Recognition using OpenCV
Introduction
What is face recognition? Or what is recognition? When you look at an apple fruit, your mind immediately tells you that this is an apple fruit. This process, your mind telling you that this is an apple fruit is recognition in simple words. So what is face recognition then? I am sure you have guessed it right. When you look at your friend walking down the street or a picture of him, you recognize that he is your friend. Interestingly when you look at your friend or a picture of him you look at his face first before looking at anything else. Ever wondered why you do that? This is so that you can recognize him by looking at his face. Well, this is you doing face recognition.
But the real question is how does face recognition works? It is quite simple and intuitive. Take a real life example, when you meet someone first time in your life you don't recognize him, right? While he talks or shakes hands with you, you look at his face, eyes, nose, mouth, color and overall look. This is your mind learning or training for the face recognition of that person by gathering face data. Then he tells you that his name is Paulo. At this point your mind knows that the face data it just learned belongs to Paulo. Now your mind is trained and ready to do face recognition on Paulo's face. Next time when you will see Paulo or his face in a picture you will immediately recognize him. This is how face recognition work. The more you will meet Paulo, the more data your mind will collect about Paulo and especially his face and the better you will become at recognizing him.
Now the next question is how to code face recognition with OpenCV, after all this is the only reason why you are reading this article, right? OK then. You might say that our mind can do these things easily but to actually code them into a computer is difficult? Don't worry, it is not. Thanks to OpenCV, coding face recognition is as easier as it feels. The coding steps for face recognition are same as we discussed it in real life example above.
•	Training Data Gathering: Gather face data (face images in this case) of the persons you want to recognize
•	Training of Recognizer: Feed that face data (and respective names of each face) to the face recognizer so that it can learn.
•	Recognition: Feed new faces of the persons and see if the face recognizer you just trained recognizes them.


 








OpenCV comes equipped with built in face recognizer, all you have to do is feed it the face data.
OpenCV Recognizers
OpenCV has three built in face recognizers and thanks to OpenCV's clean coding; you can use any of them by just changing a single line of code. Below are the names of those face recognizers and their OpenCV calls.
1.	EigenFaces Face Recognizer Recognizer - cv2.face.createEigenFaceRecognizer()
2.	FisherFaces Face Recognizer Recognizer - cv2.face.createFisherFaceRecognizer()
3.	Local Binary Patterns Histograms (LBPH) Face Recognizer - cv2.face.createLBPHFaceRecognizer()
We have got three face recognizers but do you know which one to use and when? Or which one is better? I guess not. So why not go through a brief summary of each, what you say? I am assuming you said yes :) So let's dive into the theory of each.
EigenFaces Face Recognizer
This algorithm considers the fact that not all parts of a face are equally important and equally useful. When you look at some one you recognize him/her by his distinct features like eyes, nose, cheeks, forehead and how they vary with respect to each other. So you are actually focusing on the areas of maximum change (mathematically speaking, this change is variance) of the face. For example, from eyes to nose there is a significant change and same is the case from nose to mouth. When you look at multiple faces you compare them by looking at these parts of the faces because these parts are the most useful and important components of a face. Important because they catch the maximum change among faces, change the helps you differentiate one face from the other. This is exactly how EigenFaces face recognizer works.
EigenFaces face recognizer looks at all the training images of all the persons as a whole and try to extract the components which are important and useful (the components that catch the maximum variance/change) and discards the rest of the components. This way it not only extracts the important components from the training data but also saves memory by discarding the less important components. These important components it extracts are called principal components. Below is an image showing the principal components extracted from a list of faces.


Principle components
We can see that principal components actually represent faces and these faces are called eigen faces and hence the name of the algorithm.
 
So this is how EigenFaces face recognizer trains itself (by extracting principal components). Remember, it also keeps a record of which principal component belongs to which person. One thing to note in above image is that Eigenfaces algorithm also considers illumination as an important component.
Later during recognition, when you feed a new image to the algorithm, it repeats the same process on that image as well. It extracts the principal component from that new image and compares that component with the list of components it stored during training and finds the component with the best match and returns the person label associated with that best match component.

FisherFaces Face Recognizer
This algorithm is an improved version of EigenFaces face recognizer. Eigenfaces face recognizer looks at all the training faces of all the persons at once and finds principal components from all of them combined. By capturing principal components from all the of them combined you are not focusing on the features that discriminate one person from the other but the features that represent all the persons in the training data as a whole.
This approach has drawbacks, for example, images with sharp changes (like light changes which is not a useful feature at all) may dominate the rest of the images and you may end with features that are from external source like light and are not useful for discrimination at all. In the end, your principal components will represent light changes and not the actual face features.
Fisherfaces algorithm, instead of extracting useful features that represent all the faces of all the persons, it extracts useful features that discriminate one person from the others. This way features of one person do not dominate over the others and you have the features that discriminate one person from the others.
 
a)	FisherFaces
You can see that features extracted actually represent faces and these faces are called fisher faces and hence the name of the algorithm.
One thing to note here is that even in Fisherfaces algorithm if multiple persons have images with sharp changes due to external sources like light they will dominate over other features and affect recognition accuracy.
Local Binary Patterns Histograms (LBPH) Face Recognizer
We know that Eigenfaces and Fisherfaces are both affected by light and in real life we can't guarantee perfect light conditions. LBPH face recognizer is an improvement to overcome this drawback.
Idea is to not look at the image as a whole instead find the local features of an image. LBPH alogrithm try to find the local structure of an image and it does that by comparing each pixel with its neighboring pixels.
Take a 3x3 window and move it one image, at each move (each local part of an image), compare the pixel at the center with its neighbor pixels. The neighbors with intensity value less than or equal to center pixel are denoted by 1 and others by 0. Then you read these 0/1 values under 3x3 window in a clockwise order and you will have a binary pattern like 11100011 and this pattern is local to some area of the image. You do this on whole image and you will have a list of local binary patterns.
LBP Labeling 
 


Sample Histogram
So in the end you will have one histogram for each face image in the training data set. That means if there were 100 images in training data set then LBPH will extract 100 histograms after training and store them for later recognition. Remember, algorithm also keeps track of which histogram belongs to which person.
Later during recognition, when you will feed a new image to the recognizer for recognition it will generate a histogram for that new image, compare that histogram with the histograms it already has, find the best match histogram and return the person label associated with that best match histogram. 
 



LBP Faces
Local Binary Pattern (LBP) is a simple yet very efficient texture operator which labels the pixels of an image by thresholding the neighbourhood of each pixel and considers the result as a binary number.
 


Coding the Face Recognition using opencv
The Face Recognition process in this tutorial is divided into three steps.
1.	Prepare training data: In this step we will read training images for each person/subject along with their labels, detect faces from each image and assign each detected face an integer label of the person it belongs to.
2.	Train Face Recognizer: In this step we will train OpenCV's LBPH face recognizer by feeding it the data we prepared in step 1.
3.	Testing: In this step we will pass some test images to face recognizer and see if it predicts them correctly.
Import Required Modules
Before starting the actual coding we need to import the required modules for coding. So let's import them first.
•	cv2: is OpenCV module for Python which we will use for face detection and face recognition.
•	os: We will use this Python module to read our training directories and file names.
•	numpy: We will use this module to convert Python lists to numpy arrays as OpenCV face recognizers accept numpy arrays.
Training Data
The more images used in training the better. Normally a lot of images are used for training a face recognizer so that it can learn different looks of the same person, for example with glasses, without glasses, laughing, sad, happy, crying, with beard, without beard etc. 
So our training data consists of total 2 persons with 12 images of each person. All training data is inside training_data folder.
	
Prepare training data
OpenCV face recognizer accepts data in a specific format. It accepts two vectors, one vector is of faces of all the persons and the second vector is of integer labels for each face so that when processing a face the face recognizer knows which person that particular face belongs too.
Then the prepare data step will produce following face and label vectors.
FACES                        LABELS

person1_img1_face              1
person1_img2_face              1
person2_img1_face              2
person2_img2_face              2

I am using OpenCV's LBP face detector. I convert the image to grayscale because most operations in OpenCV are performed in gray scale, then on line 8 I load LBP face detector using cv2.CascadeClassifier class. After that on line 12 I use cv2.CascadeClassifier class' detectMultiScale method to detect all the faces in the image. on line 20, from detected faces I only pick the first face because in one image there will be only one face (under the assumption that there will be only one prominent face). As faces returned by detectMultiScale method are actually rectangles (x, y, width, height) and not actual faces images so we have to extract face image area from the main image. So on line 23 I extract face area from gray image and return both the face image area and face rectangle.

Train Face Recognizer
As we know, OpenCV comes equipped with three face recognizers.
1.	EigenFace Recognizer: This can be created with cv2.face.createEigenFaceRecognizer()
2.	FisherFace Recognizer: This can be created with cv2.face.createFisherFaceRecognizer()
3.	Local Binary Patterns Histogram (LBPH): This can be created with cv2.face.LBPHFisherFaceRecognizer()
Prediction
This is where we actually get to see if our algorithm is actually recognizing our trained subjects's faces or not. We will take two test images of our celeberities, detect faces from each of them and then pass those faces to our trained face recognizer to see if it recognizes them.







Steps of coding
•	Preparing training data
	1)	Get the directories of the data path
		dirs = os.listdir(data_folder_path)
	2)	create lists to store faces and labels
	3)	extract label number from directory name
		label = int(dir_name.replace(“img", ""))
•	Build image path and read the image
	image = cv2.imread(image_path)
	face,rect = detect_face(image)
•	Form an image window to show the image and detect the face using rectangle around the face.
	cv2.imshow(Training on image…,image)
•	Detected face will be added to the list of faces and label number will be added in the label list.
•	And it will show the total no of detected faces for training.
•	Detect face from the data path of test data.
•	Convert test images into grey scale.
•	Load OpenCV face detector
	CascadeClassifier
•	Detect multiscale images and the extract the face area.
	(x,y,w,h)=faces[0]   { where x,y,w,h is defining the position of the image }
•	Return only the face part of the image.
•	At last after the prediction , it will return the faces of test data and the predicted subject corresponding to the label.
	cv2.rectangle(img,(x+y),(x+w,y+h),(0,255,0),2)
	cv2.putText(img,text,(x,y)cv2.FONT_HERSHEY_PLAIN,1.5,(0,255,0),2)
•	Now show the image 
cv2.imshow(subjects[1], cv2.resize(predicted_img1, (400, 500)))
cv2.imshow(subjects[2], cv2.resize(predicted_img2, (400, 500)))
















Application
a)	Financial services
b)	Payment Service
c)	Online Membership Verification
d)	Ride Sharing
e)	Renting Service
f)	Online Education

















Conclusion
In our project, we used OpenCV library and cascade classifier for training and detecting the faces as it is the fastest classifier to detect the faces in OpenCV. We used LBPH faces recognizer to recognize the test images on the basis of the trained recognizer. 
