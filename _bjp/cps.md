---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-12
featured: true
toc:
  - name: Problems
title: '[CPS] Core library'
description: 'making the library for the cps'
img:
importance: 2
---

# Introduction 

## CPS ROS 

## CPS API


## CPS Core 

* `Environment` : 필요한 정보들을 모두 저장.  
* `Algorithm`  : 

### Classes 📌📌📌 

* *CPSCore* :  
* *CPSRobot* :
* *Group* : 
* *Target* : 


# Environment

### 📌 CPSCore 
CPSCore (self) contains the following components

####  Variables
- `all` :   (dictionary of all items in the environment ({`cps_object.id` : CPSObject() })
- `robots` (dictionary of robots ({`robot.id` : CPSRobot() })
- `targets` :  (dictionary of targets ({`target.id` : Target() })
- `groups`  :  (dictionary of groups ({`group.id` : Group() })


####  Methods


### 📌 CPSObject 

####  Variables
- `id` : the id for the object 
- `group_ids` : list of groups that the object is in.


---

### 📌 CPSRobot (CPSObject)
* `pos` : position of the robot, numpy.ndarray (3,) shape

### 📌 CPSDrone (CPSRobot)

####  Variables
No additional variables yet 

#### Methods
* `update_from_ros_data` : update the `pos` variable


### 📌 CPSRosbot (CPSRobot)

####  Variables
No additional variables yet 

#### Methods
* `update_from_ros_data` : update the `pos` variable



---

### 📌 Group (CPSObject)


####  Variables
* `members` : (dictionary of members ({`object.id` : CPSObject() })


### 📌 RobotGroup (Group)




### 📌 DroneGroup (RobotGroup)



### 📌 UGVGroup (RobotGroup)



### 📌 DroneAndUGVGroup (RobotGroup)




---

### 📌 Path 


---

# Algorithm

