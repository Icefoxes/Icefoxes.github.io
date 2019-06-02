---
layout:       post
title:        "Useful WindowAPI"
subtitle:     "Useful WindowAPI"
date:         2016-01-06 12:00:00
author:       "Kuo"
header-img:   "img/post-bg-android.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - .NET
---

#### Windows API
```C#
[DllImport("kernel32.dll")]
public static extern uint GetTickCount();

[DllImport("user32.dll",EntryPoint="FindWindowEx",SetLastError=true)]
public static extern IntPtr FindWindowEx(IntPtr hwndParent,IntPtr hwndChilidAfter,string ipzClass,string ipzWindow);

[DllImport("user32.dll",EntryPoint="FindWindow",SetLastError=true)]
public static extern IntPtr FindWindow(string lpClassName,string lpWindowName);

[DllImport("user32.dll",EntryPoint="PostMessage",SetLastError=true)]
public static extern bool PostMessage(IntPtr hwnd,IntPtr wMsg,IntPtr wParam,IntPtr lParam);

[DllImport("user32.dll",EntryPoint="SetFocus",SetLastError=true)]
public static extern IntPtr SetFocus(IntPtr hwnd);

[DllImport("user32.dll",EntryPoint="SendMessage",SetLastError=true)]
public static extern IntPtr SendMessage(IntPtr hwnd,IntPtr wMsg,IntPtr wParam,string lParam);
```