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
    - .NET
---

### VS Snippet

Use *propdp* create dependency property

### create style for datagrid header

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
### Add Menu Into DataGrid

```XML
<DataGrid.ContextMenu>
    <ContextMenu>
        <MenuItem Header="Show Item" Command="{Binding Reframe}"/>
        <MenuItem Header="Export" Command="{Binding Export}"/>
    </ContextMenu>
</DataGrid.ContextMenu>
```

### Convert Event To Command

```XML
<TabControl>
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="SelectionChanged">
            <i:InvokeCommandAction Command="{Binding SelectionChaged}" />
        </i:EventTrigger>
    </i:Interaction.Triggers>
</TabControl>
```

### DataGrid EditingElementStyle And ElementStyle

```XML
<DataGridComboBoxColumn.EditingElementStyle>
<Style TargetType="ComboBox">
    <Setter Property="ItemsSource" Value="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type metro:MetroWindow}}, Path=DataContext.PropetyTypes}"></Setter>
    <Setter Property="IsEditable" Value="True"></Setter>
    <Setter Property="Text" Value="{Binding Path=Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"></Setter>
</Style>
</DataGridComboBoxColumn.EditingElementStyle>
<DataGridComboBoxColumn.ElementStyle>
<Style TargetType="ComboBox">
    <Setter Property="ItemsSource" Value="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type metro:MetroWindow}}, Path=DataContext.PropetyTypes}"></Setter>
    <Setter Property="Text" Value="{Binding Path=Name, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}"></Setter>
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

### Binding Visibility to DataColumn
DataColumn doesnot exsit in Visual Tree, so it requires a proxy class to implement the binding
```CSharp
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

    public static readonly DependencyProperty DataProperty = DependencyProperty.Register("Data", typeof(object),  typeof(BindingProxy), new UIPropertyMetadata(null));
}

```

```XML
<UserControl.Resources>
    <local:BindingProxy x:Key="proxy" Data="{Binding}" />
</UserControl.Resources>

<DataGridTemplateColumn Visibility="{Binding Data.IsVisible,  Source={StaticResource proxy},
    Converter={StaticResource BooleanToVisibilityConverter}}">
```