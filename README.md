<img width="108" alt="image" src="https://user-images.githubusercontent.com/91287933/225829636-0a8417f3-e44b-48ed-aa78-3218c20f3675.png">
<span style=“font-weight:bold”> Automated pothole detection and reporting system using Computer Vision</span>

Abstract
The rapid growth of remote sensing technologies, superior computing power, and machine learning techniques can help local governments to make pothole detection and reporting more efficient. We proposed an automated pothole detection and reporting system that utilizes an edge computing device installed on a garbage truck to detect and report potholes automatically. The device captures images of the road surface, and object detection techniques are used to detect potholes. If a pothole is detected, the device sends the road surface image, GPS latitude, and longitude to the server. The server counts the number of potholes and prioritizes them based on severity. The system provides a user-friendly interface for visualizing the potholes' locations on a map. The proposed system reduces the need for manual reporting, minimizes time and resources for road maintenance, and increases road safety. The system also positively impacts the environment by utilizing an existing garbage truck for data collection and minimizing unnecessary maintenance, reducing costs and carbon footprint. Overall, the proposed system provides a safer, more efficient, and environmentally friendly method for tracking road conditions.

System Architecture



<img width="276" alt="image" src="https://user-images.githubusercontent.com/91287933/225829813-1b813563-eeba-4436-a525-d64c978f4861.png">


In the initial stage of our system architecture, we employed edge computing to enhance the garbage trucks in cities by equipping them with a Raspberry Pi 4+ edge device. The device was programmed to capture images of the road and classify them as "Pothole" or "Normal Road" utilizing the EfficientNet0.tlite model, which was retrained on pothole data. Upon detecting a pothole in an image, the device employed an SCP call to transmit data, including the image, location, and date time, to a Pothole Queue in the central server. We opted for edge computing because the device required offline capabilities to function in areas with dead zones, low latency to minimize bottlenecks in the central server, and scalability to enable the incorporation of more garbage trucks with edge devices into cities.
In the second stage, we utilized a Linux server that was equipped with 2 GPUs to process the Pothole Queue. To identify every pothole in each image, we employed an RCNN (Regional Convolutional Neural Network) and employed a Selective Search algorithm to segment the image using texture, color, and other features. We then utilized the VGG16 model, which had 8 frozen layers with an output layer of 2 neurons, to examine the top 2000 region proposals. During our training process, we used an annotated pothole dataset, wherein each image was run in our selective search algorithm to generate region proposals. We then compared the results to the ground truth using the IOU (Intersection Over Overlap) metric and trained our VGG16 model solely on the potholes with an IOU score above 0.7. Following classification, if a proposed region contained a pothole, the image was subjected to a Non-Maxima Suppression algorithm to minimize the number of squares so that there would be only one square for each detected object. The data from each image was then subjected to a severity algorithm that utilized thresholds derived from our training data to determine the severity of the road based on the number of potholes detected.
In the final stage, we utilized Dr. Sada's Django template to create a UI that displayed our results. Prior to processing, the front end showed all the identified potholes on an OpenStreetMap, with grey markers indicating their location. After processing a batch of images, the markers changed to green, and all the images of potholes were displayed with rectangles around each identified pothole in the image.

<img width="485" alt="image" src="https://user-images.githubusercontent.com/91287933/225830084-5eb71824-a0ea-4ee7-8c99-9963a70b2237.png">
<img width="380" alt="image" src="https://user-images.githubusercontent.com/91287933/225830108-7a5e5424-c722-4d28-8384-999947613e83.png">


How to use the system 


