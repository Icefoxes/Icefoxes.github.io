---
layout:       post
title:        "Get Gravity Coordinates in CATIA"
subtitle:     "Get Gravity Coordinates in CATIA"
date:         2015-07-09 12:00:00
author:       "Gary"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - CATIA
---

#### Get Gravity Coordinates in CATIA

```C#
private List<int> Get_Coordinate(Product SelectedProduct)
{
  object[] coordinate = new object[3];
  Inertia inertia = SelectedProduct.GetTechnologicalObject("Inertia") as Inertia;
  if (inertia.Density > 0)
     inertia.GetCOGPosition(coordinate);
  else
     logger.Warn(SelectedProduct.get_Name() + ",its Inertia is 0");
  // if density = 0, return string is [0,0,0]
  return (coordinate.ToList().Select(coor => Convert.ToInt32(Convert.ToDouble(coor) * 1000))).ToList();
}
```