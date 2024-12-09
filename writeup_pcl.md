# ID_S1_EX2 examples of vehicles and stable features in PCL

## Examples of vehicles in point cloud visualization:

<img src="img_writeup_pcl\Screenshot 2024-12-09 011220.jpg"/>
<img src="img_writeup_pcl\Screenshot 2024-12-09 011306.jpg"/>
<img src="img_writeup_pcl\Screenshot 2024-12-09 011453.jpg"/>
<img src="img_writeup_pcl\Screenshot 2024-12-09 011606.jpg"/>
<img src="img_writeup_pcl\Screenshot 2024-12-09 011857.jpg"/>
<img src="img_writeup_pcl\Screenshot 2024-12-09 012011.jpg"/>
<img src="img_writeup_pcl\Screenshot 2024-12-09 012058.jpg"/>

## Observation on features and stability

1. Given the concentric nature of the lidar-collected point cloud:
    - vehicles closer are more visible then the ones further away
    - vertical surfaces have a denser point cloud then horizontal ones. For vehicles, this means the sides are more visible than the top, or even the bonet
    - the sides of the vehicle facing the EGO car are visible, the other sides are usually invisible (in cover). Same goes for any vehicle feature behind other object or vehicles

2. As a consequence of all the above:
    - the most visible features of most vehicles are the front bumpers or the trunk (depending on direction of travel and in front / behind). Considering the linear structure of roads, this is most common. Cars far can mostly be seen as such
    - cars very close can be seen from the side

3. Other observations
    - windows are less visible than the body and the columns of the vehicles
    - wheels and mirrors are also visible (but smaller objects, so less visible in distance)
    - in general it is unlikely to get a point cloud representation of the "whole" vehicle, it is almost always one side, and just a couple of features (the boxy silhuette and size) that is likely the base of the detection.