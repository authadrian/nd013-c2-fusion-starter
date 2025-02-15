# Writeup: Track 3D-Objects Over Time

Please use this starter template to answer the following questions:

### 1. Write a short recap of the four tracking steps and what you implemented there (filter, track management, association, camera fusion). Which results did you achieve? Which part of the project was most difficult for you to complete, and why?

__Step 1 _ Kalman Filter__
I had to set up the (Kalman) Filter class as the basic building block of tracking and sensor fusion system. At this point, without track management and data association, it would only work tracking a single track. The only difference from the course materials was the dimensionality, having 3D objects and therefore 6D matrices, instead of 2D / 4D. It was quite straigthforward - but I got into trouble at step 4, keep using get_H instead of the more general get_hx.

<img src="img_writeup_fusion\RMSE step 1.jpg"/>

__Step 2 _ Track Management__
Had to set up track management by completing the Track and Trackmanagement classes. It involved functions to initialize and delete tracks, keeping track of track states and track score. While the logic of states and scores is not too hard, for me personally dealing with transformation matrices is always a bit harder - I would just need more practice with it.

RMSE is quite high in this step and boxes do not fit the car too well, because lidar detections contain a y-offset we do not compensate for (and the KAlman Filter itself cannot).

<img src="img_writeup_fusion\RMSE step 2.jpg"/>

__Step 3 Association__
In this step I have implemented the Association class that allowed for multi-target tracking. It used simple, single nearest data association algorithm to determine the closest measurement and track in the association matrix, Mahalanobis distance to determine distance and gating to speed up calculations by "ignoring" unlikely pairings.

It was quite straigforward step as it had not much difference from the excercises in course material.
The results, which are only based on lidar measurements, are already pretty good, the tracks are stable and boxes fit the cars.

<img src="img_writeup_fusion\RMSE step 3.jpg"/>


__Step 4 Camera Fusion__
Implementing the nonlinear camera measurement model was by far the trickiest poart for me. While implementing the functions was not that problematic, I ended up debugging for a long time, as my tracks got deleted at each frame after camera measurements were taken into account. Could not figure that the issue was in my filter.py file, using get_H instead of get_hx...

RMSE values were somewhat lower with camera fusion implemented, with values of [.15, .12, .19] vs [.17, .10, .13] respectively. Judged on this single multi-tracking scenario, the gain from fusion is limited. 

<img src="img_writeup_fusion\RMSE step 4.jpg"/>




### 2. Do you see any benefits in camera-lidar fusion compared to lidar-only tracking (in theory and in your concrete results)? 

In theory, with additional information from multiple sensors should always improve the robustness and precision of the system.
(ideas of comparative strenghts and weaknesses:
- cam better for classification(is it a bicycle or a pedestrian? but worse at localization))

In the excercise, I have seen some limited improvement in the precision: RMSE values were somewhat lower with camera fusion implemented, with values of [.15, .12, .19] vs [.17, .10, .13] respectively.


### 3. Which challenges will a sensor fusion system face in real-life scenarios? Did you see any of these challenges in the project?

- environmental challenges. Certain weather conditions severly limit both camera (darkness, fog, glare), and lidar (precipition, icying). Newer lidar systems could deal with this better. In a real-life application I would both have to add redundancy (more / other sensors) and some fallback mechanism when all those fail (fallback may not be part of the fusion system).
I have seen some ghost measurements in step 4 of the project, but the system handled it well - similar as what I would expect for same measurements I would like to ignore in real life (droplets, maybe a falling leaf, etc).

- occlusions (can radar detect that) and handling a large number of objects. Lidar, Camera and even Radar work on a line-of-sight principle so keeping track of occluded objects is harder. Some advanced techiques could help (multi-path radar reflections of high-res 4D radars, AI-driven prediction models that can pick up on subtle changes, like shadows). Dealing with large number of objects (like a crowd of pedestrians) is also hard and computationally heavy. Some novel techniques model crowds as a single crowd entity instead of a lot is individual objects (like Wayve's PRISM-! $D scene reconstruction).

- real-time computation requirement. On-board computational requirement coupled with the requirement of making correct predictions in real time limits the complexity of the fusion system. This was not the case on this project, as the data could be processed frame-by-frame at any speed.

- sensor calibration challenges. Sensor often have to be re-calibrated after some use, but that cannot happen all the time for operational reasons. As calibration gets slightly off, the quality of the fusion model can decrease. Not applicable to this project.




### 4. Can you think of ways to improve your tracking results in the future?

a) all the recommendations for the project to stand out could be considered:
    - adding a more robust association system, like GNN or JPDA
    - fine-tune parametrization
    - improving the models for each sensor to provide better detections as input for fusion system
    - using non-linear motion model that better describes vehicle movement

b) Adding more sensors for additional information, redundancy, field-of-view, and so on.

c) taking a different, early fusion approach where you feed all your raw sensor input into a singular ML model to make predictions directly in the 3D space. While this have high potential (more rich input to the final system that makes predictions, less computation using one single model instead of multiple separate ones), it is also much harder to build, as it has to be developed from the ground up, mostly ignoring a lot of innovation done by sensor HW companies who are building better and better models for their own sensors.

