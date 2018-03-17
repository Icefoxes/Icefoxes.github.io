---
layout:       post
title:        "WPF watermark"
subtitle:     "WPF watermark implement"
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


添加 `IsMonitoring`, `WatermarkText`, `TextLength`, `HasText`四个依赖属性， 其中当`IsMonitoring`变化时动态的为`TextBox`和`Password`对应的`TextChanged`事件增加新的实现联系起来

```C#
public class WaterMarkTextHelper : DependencyObject
{
    #region Attached Properties

    public static bool GetIsMonitoring(DependencyObject obj)
    {
        return (bool)obj.GetValue(IsMonitoringProperty);
    }

    public static void SetIsMonitoring(DependencyObject obj, bool value)
    {
        obj.SetValue(IsMonitoringProperty, value);
    }

    public static readonly DependencyProperty IsMonitoringProperty =
        DependencyProperty.RegisterAttached("IsMonitoring", typeof(bool), typeof(WaterMarkTextHelper), new UIPropertyMetadata(false, OnIsMonitoringChanged));

    
    public static bool GetWatermarkText(DependencyObject obj)
    {
        return (bool)obj.GetValue(WatermarkTextProperty);
    }

    public static void SetWatermarkText(DependencyObject obj, string value)
    {
        obj.SetValue(WatermarkTextProperty, value);
    }

    public static readonly DependencyProperty WatermarkTextProperty =
        DependencyProperty.RegisterAttached("WatermarkText", typeof(string), typeof(WaterMarkTextHelper), new UIPropertyMetadata(string.Empty));

    
    public static int GetTextLength(DependencyObject obj)
    {
        return (int)obj.GetValue(TextLengthProperty);
    }

    public static void SetTextLength(DependencyObject obj, int value)
    {
        obj.SetValue(TextLengthProperty, value);

        if (value >= 1)
            obj.SetValue(HasTextProperty, true);
        else
            obj.SetValue(HasTextProperty, false);
    }

    public static readonly DependencyProperty TextLengthProperty =
        DependencyProperty.RegisterAttached("TextLength", typeof(int), typeof(WaterMarkTextHelper), new UIPropertyMetadata(0));

    #endregion

    #region Internal DependencyProperty

    public bool HasText
    {
        get { return (bool)GetValue(HasTextProperty); }
        set { SetValue(HasTextProperty, value); }
    }
    
    private static readonly DependencyProperty HasTextProperty =
        DependencyProperty.RegisterAttached("HasText", typeof(bool), typeof(WaterMarkTextHelper), new FrameworkPropertyMetadata(false));

    #endregion

    #region Implementation

    static void OnIsMonitoringChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        if (d is TextBox)
        {
            TextBox txtBox = d as TextBox;

            if ((bool)e.NewValue)
                txtBox.TextChanged += TextChanged;
            else
                txtBox.TextChanged -= TextChanged;
        }
        else if (d is PasswordBox)
        {
            PasswordBox passBox = d as PasswordBox;

            if ((bool)e.NewValue)
                passBox.PasswordChanged += PasswordChanged;
            else
                passBox.PasswordChanged -= PasswordChanged;
        }
    }

    static void TextChanged(object sender, TextChangedEventArgs e)
    {
        TextBox txtBox = sender as TextBox;
        if (txtBox == null) return;
        SetTextLength(txtBox, txtBox.Text.Length);
    }

    static void PasswordChanged(object sender, RoutedEventArgs e)
    {
        PasswordBox passBox = sender as PasswordBox;
        if (passBox == null) return;
        SetTextLength(passBox, passBox.Password.Length);
    }
    
    #endregion
}
```
接下来重写`Textbox` `PasswordBox`的`ControlTemplate`


`Textbox`的`ControlTemplate`, 直接定义`IsMonitoring`和`WatermarkText`，定义一个`Opacity`为`0.8`的`TextBolck`,Binding在WatermakrText上

```XML
<Style x:Key="WaterMarkTextBox" TargetType="{x:Type TextBox}">
    <Setter Property="local:WaterMarkTextHelper.IsMonitoring" Value="True"/>
    <Setter Property="local:WaterMarkTextHelper.WatermarkText" Value="Username" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type TextBox}">
                <ControlTemplate.Resources>
                    <Storyboard x:Key="enterGotFocus" >
                        <DoubleAnimation Duration="0:0:0.4" To=".2" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>
                    <Storyboard x:Key="exitGotFocus" >
                        <DoubleAnimation Duration="0:0:0.4" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>

                    <Storyboard x:Key="enterHasText" >
                        <DoubleAnimation Duration="0:0:0.4" From=".2" To="0" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>
                    <Storyboard x:Key="exitHasText" >
                        <DoubleAnimation Duration="0:0:0.4" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>
                </ControlTemplate.Resources>
                <Border Name="Bd" 
                        Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <Grid>
                        <ScrollViewer x:Name="PART_ContentHost" VerticalAlignment="Center" Margin="1" />
                        <TextBlock 
                            x:Name="Message" 
                            Text="{TemplateBinding local:WaterMarkTextHelper.WatermarkText}"
                            FontStyle="Italic"
                            Foreground="Gray" 
                            IsHitTestVisible="False" 
                            FontFamily="Calibri" 
                            Opacity="0.8" 
                            HorizontalAlignment="Left" 
                            VerticalAlignment="Center"
                            Margin="6,0,0,0"/>
                    </Grid>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsEnabled" Value="false">
                        <Setter 
                            TargetName="Bd" 
                            Property="Background" 
                            Value="{DynamicResource {x:Static SystemColors.ControlBrushKey}}"/>
                        <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.GrayTextBrushKey}}"/>
                    </Trigger>

                    <MultiTrigger>
                        <MultiTrigger.Conditions>
                            <Condition Property="local:WaterMarkTextHelper.HasText" Value="False"/>
                            <Condition Property="IsFocused" Value="True"/>
                        </MultiTrigger.Conditions>
                        <MultiTrigger.EnterActions>
                            <BeginStoryboard Storyboard="{StaticResource enterGotFocus}"/>
                        </MultiTrigger.EnterActions>
                        <MultiTrigger.ExitActions>
                            <BeginStoryboard Storyboard="{StaticResource exitGotFocus}"/>
                        </MultiTrigger.ExitActions>
                    </MultiTrigger>

                    <Trigger Property="local:WaterMarkTextHelper.HasText" Value="True">
                        <Trigger.EnterActions>
                            <BeginStoryboard Storyboard="{StaticResource enterHasText}"/>
                        </Trigger.EnterActions>
                        <Trigger.ExitActions>
                            <BeginStoryboard Storyboard="{StaticResource exitHasText}"/>
                        </Trigger.ExitActions>
                    </Trigger>

                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```
`Passwordbox`的`ControlTemplate`

```XML
<Style TargetType="{x:Type PasswordBox}">
    <Setter Property="local:WaterMarkTextHelper.IsMonitoring" Value="True"/>
    <Setter Property="local:WaterMarkTextHelper.WatermarkText" Value="Password" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type PasswordBox}">
                <ControlTemplate.Resources>
                    <Storyboard x:Key="enterGotFocus" >
                        <DoubleAnimation Duration="0:0:0.4" To=".2" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>
                    <Storyboard x:Key="exitGotFocus" >
                        <DoubleAnimation Duration="0:0:0.4" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>

                    <Storyboard x:Key="enterHasText" >
                        <DoubleAnimation Duration="0:0:0.4" From=".2" To="0" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>
                    <Storyboard x:Key="exitHasText" >
                        <DoubleAnimation Duration="0:0:0.4" Storyboard.TargetProperty="Opacity" Storyboard.TargetName="Message"/>
                    </Storyboard>
                </ControlTemplate.Resources>
                <Border Name="Bd" 
                        Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <Grid>
                        <ScrollViewer x:Name="PART_ContentHost" VerticalAlignment="Center" Margin="1" />
                        <TextBlock x:Name="Message" FontStyle="Italic"
                                   Text="{TemplateBinding local:WaterMarkTextHelper.WatermarkText}" 
                                   Foreground="Gray" IsHitTestVisible="False" FontFamily="Calibri"
                                   Opacity="0.8" HorizontalAlignment="Left" VerticalAlignment="Center" Margin="6,0,0,0"/>
                    </Grid>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsEnabled" Value="false">
                        <Setter TargetName="Bd" Property="Background" Value="{DynamicResource {x:Static SystemColors.ControlBrushKey}}"/>
                        <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.GrayTextBrushKey}}"/>
                    </Trigger>
                    <Trigger Property="IsEnabled" Value="True">
                        <Setter Property="Opacity" Value="1" TargetName="Bd"/>
                    </Trigger>
                    <MultiTrigger>
                        <MultiTrigger.Conditions>
                            <Condition Property="local:WaterMarkTextHelper.HasText" Value="False"/>
                            <Condition Property="IsFocused" Value="True"/>
                        </MultiTrigger.Conditions>
                        <MultiTrigger.EnterActions>
                            <BeginStoryboard Storyboard="{StaticResource enterGotFocus}"/>
                        </MultiTrigger.EnterActions>
                        <MultiTrigger.ExitActions>
                            <BeginStoryboard Storyboard="{StaticResource exitGotFocus}"/>
                        </MultiTrigger.ExitActions>
                    </MultiTrigger>

                    <Trigger Property="local:WaterMarkTextHelper.HasText" Value="True">
                        <Trigger.EnterActions>
                            <BeginStoryboard Storyboard="{StaticResource enterHasText}"/>
                        </Trigger.EnterActions>
                        <Trigger.ExitActions>
                            <BeginStoryboard Storyboard="{StaticResource exitHasText}"/>
                        </Trigger.ExitActions>
                    </Trigger>

                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```
