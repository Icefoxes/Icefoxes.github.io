---
layout:       post
title:        "MVVMLight"
subtitle:     "Basic MVVMLight implementation"
date:         2018-01-05 12:00:00
author:       "Gary"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
comments:     true
tags:
    - WPF
---

#### MVVMLight

- 每个`ViewModel`都继承自`ViewModelBase`，绑定的的属性需要实现 `RaisePropertyChanged`

- 每个绑定的`Command`使用`ICommand`暴露，在`ViewModel`的构造函数里用`RelayCommand`指向实现


```C#
public ICommand Export { get; private set; }

private void ExportExcute(){ }

public Class()
{
     Export = new RelayCommand(() => ExportExcute(), () => true);
}
```
- `Messenger`实现的消息机制
- `SimpleIoc` 实现`IOC`容器
