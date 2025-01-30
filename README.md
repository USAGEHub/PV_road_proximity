# Description

The tool determines whether a roof part of known aspect is visible from the surrounding roads or not, within a fixed maximum distance of 50 meters. Closeness is determined via a viewshed analysis based on a set of viewpoints homogeneously distributed along the whole road network of the area of interest and assumes the height of the observer to be 1.80 meters. A roof part is marked as “visible” if at least 20% of its surface is visible according to the viewshed analysis.

# Input data requirements

* Segmented roof parts with aspect attribute
* Road network
* Digital Surface Model (DSM)
* Area of Interest (AoI)
* Model parameters:
   * maximum visibility distance (default = 50 meters)
   * viewpoint height (default = 1.80 meters)
   * roof visibility threshold (default = 20%)

# Output data requirements

Segmented roof parts with additional attribute:

* visibile: roof part visibility from the road network (true/false)

# Objective

Support PV panels suitability analysis by identifying roof parts that are visible from the nearby roads, which is a relevant parameter in protected part of cities like historical downtown areas.

# Use of resource

* Import the user input parameters into the model
* Use te buffer tool on the AoI with a size equal to the “maximum visibility distance” parameter and use it to clip the road network
* use the “Points along the geometry” tool to sample the roads in points, every 10 meters
* use the “Visibility Analysis” plugin in two steps:
   * run the “Create viewpoints” tool taking as input the points previously created every 10m and the DSM;
   * use the output of this clipping as input to the step that calculates the "Viewshed"
* Run zonal statistics between the input vector layer “Road parts” and the output of the “Viewshed” tool to statistically calculate the count/sum values
* Run the field calculator, creating a new field ‘percent_visibility’ valued according to the formula = (sum/count)*100
* Run the field calculator, creating a new field of type 'String' called ‘visible’ with possible values: "yes" if percent_visibility >= threshold percentage of visibility, otherwise "no"
* Run the “Delete field” tool to delete the 'percent_visibility;_sum;_count' fields present in the output of the previous tool

--------------------------------------------------------------------------------------------------------
