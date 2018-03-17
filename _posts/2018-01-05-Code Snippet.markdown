---
layout:       post
title:        "WPF Code Snippet"
subtitle:     "WPF Code Snippet"
date:         2018-01-05 12:00:00
author:       "Gary"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - WPF
---

### VS Snippet

使用 *propdp* 快捷添加依赖属性

### 使DataGrid的内容和Header对齐

```XML
<DataGrid.CellStyle>
    <Style TargetType="DataGridCell">
        <Setter Property="TextBlock.HorizontalAlignment" Value="Center"/>
        <Setter Property="TextBlock.VerticalAlignment" Value="Center"/>
    </Style>
</DataGrid.CellStyle>

<DataGrid.ColumnHeaderStyle>
    <Style TargetType="DataGridColumnHeader">
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
    </Style>
</DataGrid.ColumnHeaderStyle>
```
### 在DataGrid里加入菜单

```XML
<DataGrid.ContextMenu>
    <ContextMenu>
        <MenuItem Header="Show Item" Command="{Binding Reframe}"/>
        <MenuItem Header="Export" Command="{Binding Export}"/>
    </ContextMenu>
</DataGrid.ContextMenu>
```

### 事件转化为命令

```XML
<TabControl>
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="SelectionChanged">
            <i:InvokeCommandAction Command="{Binding SelectionChaged}" />
        </i:EventTrigger>
    </i:Interaction.Triggers>
</TabControl>
```

### DataGrid EditingElementStyle和ElementStyle

```XML
<DataGridComboBoxColumn.EditingElementStyle>
<Style TargetType="ComboBox">
    <Setter Property="ItemsSource" Value="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type metro:MetroWindow}}, Path=DataContext.PropetyTypes}"></Setter>
    <Setter Property="IsEditable" Value="True"></Setter>
    <Setter Property="Text" Value="{Binding Path=名称, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"></Setter>
</Style>
</DataGridComboBoxColumn.EditingElementStyle>
<DataGridComboBoxColumn.ElementStyle>
<Style TargetType="ComboBox">
    <Setter Property="ItemsSource" Value="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type metro:MetroWindow}}, Path=DataContext.PropetyTypes}"></Setter>
    <Setter Property="Text" Value="{Binding Path=名称, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}"></Setter>
</Style>
</DataGridComboBoxColumn.ElementStyle>
```

### Image Button

```XML
<Button>
    <Button.Style>
        <Style TargetType="{x:Type Button}">
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="{x:Type Button}">
                        <Image Source="/Images/appbar.edit.add.png" />
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Button.Style>
</Button>
```

### 为DataColumn定义Visibility

难点：因为DataColumn不存在于Visual Tree里，所以不能使用一般的Binding方法（比如使用FindAncestor)。需要定义一个Freezable的类
```C#
public class BindingProxy : Freezable
{
    protected override Freezable CreateInstanceCore()
    {
        return new BindingProxy();
    }

    public object Data
    {
        get { return (object)GetValue(DataProperty); }
        set { SetValue(DataProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Data.
    // This enables animation, styling, binding, etc...
    public static readonly DependencyProperty DataProperty =
        DependencyProperty.Register("Data", typeof(object), 
        typeof(BindingProxy), new UIPropertyMetadata(null));
}

```
然后将其放入UserControl的顶层资源中，并且Binding住DataContext
```XML
<UserControl.Resources>
    <local:BindingProxy x:Key="proxy" Data="{Binding}" />
</UserControl.Resources>

<DataGridTemplateColumn Visibility="{Binding Data.IsVisible,  Source={StaticResource proxy},
    Converter={StaticResource BooleanToVisibilityConverter}}">
```