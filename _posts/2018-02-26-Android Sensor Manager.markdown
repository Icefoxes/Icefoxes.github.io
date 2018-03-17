---
layout:       post
title:        "Android Sensor Manager"
subtitle:     "Android Sensor Manager"
date:         2018-02-26 12:00:00
author:       "Gary"
header-img:   "img/post-bg-android.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Android
---

##### Sensor
###### Gravity Sensor
The gravity sensor provides a three dimensional vector indicating the direction and magnitude of gravity
```Kotlin
val sensorManager = applicationContext.getSystemService(Context.SENSOR_SERVICE) as SensorManager
val sensor = sensorManager.getDefaultSensor(Sensor.TYPE_GRAVITY)
```

###### Linear Accelerometer
The linear acceleration sensor provides you with a three-dimensional vector representing acceleration along each device axis, excluding gravity. You can use this value to perform gesture detection.
```Kotlin
val sensorManager = applicationContext.getSystemService(Context.SENSOR_SERVICE) as SensorManager
val sensor = sensorManager.getDefaultSensor(Sensor.TYPE_LINEAR_ACCELERATION)
```

###### Rotation Vector Sensor
The rotation vector represents the orientation of the device as a combination of an angle and an axis, in which the device has rotated through an angle θ around an axis (x, y, or z). The following code shows you how to get an instance of the default rotation vector sensor:
```Kotlin
val sensorManager = applicationContext.getSystemService(Context.SENSOR_SERVICE) as SensorManager
val sensor = sensorManager.getDefaultSensor(Sensor.TYPE_ROTATION_VECTOR)
```