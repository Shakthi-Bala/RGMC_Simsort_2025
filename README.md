# RGMC_Simsort_2025

Solution walkthrough:
1. The environment to plan pick and place is huge, So instead of making camera look various position and stiching image together, The idealogy is to get different lookup position and plan trajectory accordingly.
2. There are in total of 7 lookup positions.
3. There are 5 different regions, Firstly Static region, Next comes Orientation change region, Followed by objects placed in a box, Followed by Object change region, Lastly Out of reach region.
4. The perception model trained is Yolov4 on a custom dataset with confidence threshold of 85% and enviroment used is matlab.
5. The plan for pick and place is to obtain PointStamped data by means of Deprojection from the camera subsystem and Use inverse Kinematics for pick and place. The catch here is also the object needs to be sorted based on its type.
6. The pick and place for static region can be hardcoded, With fixed orientation for each object and place position i.e sorting done based on classIndex from the camera subsystem.
7. The pick and place for orientation change region is done by obtaining position and orientaion of the object by means of Principal Component Analysis (PCA), The idea is to obtain point cloud data from the bounding box detected by menas of Yolov4, Then obtain the centre data and apply PCA there to find the direction in which the object is lying in terms of its variance.
8. The pick and place for Object change region follows the same logic as well.
9. For the out of reach region, The trajectory is streamlined i.e 2-3 cubic splines are generated for and place, It is made sure that the spline lies above the ground surface, So individual trajectory is plannned from eachc checkpoint, This avoids spline to be generated in a wierd manner.
10. For the objects in a box region, Segmentation along with PCA is carried out for pick and place.
11. The sorting for each objects picked up is done using classIndex detected from the perception model.
