层次 

​	Solution  解决方案

 	Project

​			三种类型    Winexe (windows应用程序)  Library (类库)   exe(控制台应用程序)

​		properties   资源文件

​		references 引用

​		xaml  声明性语言

```xaml
<Window x:Class="HelloWPF.MainWindow"    //和cs里的class对应 编译后合在一起
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
        //默认名称空间
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        //  加了x 引用其内部标签也要加x  
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWPF"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        
    </Grid>
</Window>
```

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace HelloWPF
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window         //partial  分部类  和xaml对应合并
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
	
```

11.18



Winfrom 事件驱动  WPF数据驱动



控件分类    F12查看控件类型

​	布局控件  Panel	

​	内容控件  ContentControl   只能容纳一个控件或布局控件

​	带标题内容控件  内容控件可以设置标题Header 父类：HeaderedContentControl

​	条目控件 可以显示一列数据  数据类型一般相同 ItemControl

​		可以设置标题

​	特殊内容控件

​		TextBox PasswordBox TextBlock Image

​	可以引入第三方控件



XAML     构建应用程序用户界面 可扩展应用程序标记语言

命名空间语法 

​	xmlns:[前缀] = “命名空间描述”

​	自定义类或程序集映射语法   xmlns:[必选前缀] = “clr-namespace:[命名空间]；assembly = [程序集名称]”



App.config 配置文件	连接数据库 

App.xaml  系统级资源







Window

​	内容控件里只能承载一个Content

​	ShowInTaskbar="False"   是否在任务栏显示

​	WindowStartupLocation="CenterScreen"  窗体首次显示位置

​	WindowState="Maximized"  窗口显示模式  （最大化最小化）

​	Topmost="True"  显示的优先级  是否在最上层

​	Icon="asset/images/globe.jpg"  图标



RadioButton

```
GroupName="type"   分组
```



CheckBox

```
IsThreeState="True"   存在中间状态   null/true/false
```

Image

```
Source = "";
Stretch = "Stretch/Null/Uniform/UniformToFill" F12看具体属性  默认Uniform
StretchDirection 
```

Border

​	常与布局面板一起使用 

​	只能有一个子元素

​	要显示多个子元素时  内部用一个布局面板作为子元素 布局面板内在存放其他元素

```
BorderBrush="Red"      边框颜色
BorderThickness="2"    边框粗细
CornerRadius="5"       弧度
Background="Yellow"    背景色
Padding
```

下拉框

​	ComboBox 条目控件 ItemControl

​	ComboBoxItem  内容控件

```
IsDropDownOpen="True"   下拉框是否展开

获取数据进行绑定
List<MoviesModel> list = getTestList();
combobox.SelectedValuePath = "Id";
combobox.DisplayMemberPath = "Name";
combobox.Items.Clear();
combobox.ItemsSource = list;  //其他绑定方式参照vs中代码


//事件
SelectionChanged="listBox_SelectionChanged"
//注意获取时类型的转化 存入时什么类型就 as xx

```



列表框  ListBox

```
操作同上 ComboBox

SelectionMode="Multiple"  多选

两个ListBox中的项相互移动时，不适合通过ItemsSource存入  因为涉及到类型的转换 
```

DatePicker

```
自定义格式通过Style,template来实现
```

Calendar

```
DisplayMode="Decade"   首先显示的类型  
SelectionMode="MultipleRange"  选择的模式 要按住Ctrl键
```

Slider 

```
Maximum="90" Minimum="50" Value="51"
Orientation="Horizontal"   方向
TickPlacement="TopLeft"  刻度显示位置
TickFrequency="10"  刻度间隔
IsSnapToTickEnabled="True"  拖动后的值是不是整型
SmallChange="5" LargeChange="10" 拖动小步和大步的位移距离  直接点右边空白处就是一大步
IsDirectionReversed="True"  为true时值方向设置为反向
IsSelectionRangeEnabled="True" SelectionStart="66" SelectionEnd="70"   选范围
```

ProgressBar 进度条

```
Orientation="Vertical"  方向
Minimum="20" Maximum="120"   范围
Value = "30"  完成度    
IsIndeterminate="True"   ture 动态进度   false  显示值

//Dispatcher  线程任务
//线程任务投放
progressBar.Value = 0;
var max = progressBar.Maximum;
    Task.Run(() =>
    {
        for (int i = 0; i < max; i++)
        {
            progressBar.Dispatcher.Invoke(() => {
            progressBar.Value = i;
        });
    	Thread.Sleep(1000);
    }

});


```





<u>布局控件</u>

StackPanel 

​	排列成一行或者一列 可以嵌套

```
Orientation="Vertical"  默认垂直方向排列  
垂直排列时设置水平对齐  水平排列时设置垂直对齐
通过Margin设置边距
FlowDirection="LeftToRight"   水平排列时可设置item布局方向
当子元素过多导致空间不够时,多余子元素会被隐藏
```



WrapPanel 流面板  

​	子元素按顺序排列  如果空间不够 会自动换行/列

```
基本同 StackPanel
ItemHeight="30" ItemWidth="100"  设置子元素宽高
常在StackPanel中嵌套使用
```



DockPanel 停靠面板

​	先添加的子元素优先占有边界  常用于布局自适应页面

```
DockPanel.Dock="Left"  布局位置
LastChildFill="True"  最后元素是否占有所有剩余空间
```

```
<DockPanel LastChildFill="True">
        <StackPanel DockPanel.Dock="Left" Width="20" Background="Red"></StackPanel>
        <StackPanel DockPanel.Dock="Top" Height="20" Background="Yellow"></StackPanel>
        <StackPanel DockPanel.Dock="Right" Width="20" Background="Green"></StackPanel>
        <StackPanel DockPanel.Dock="Bottom" Height="20" Background="Orange"></StackPanel>
        <Grid Background="Gray"></Grid>
    </DockPanel>
```



Canvas  画布面板 坐标面板

​	定义区域，子元素的显示位置，指定相对于面板的坐标

​	两点定位  不用指定上下左右所有的位置  后面的定位会被忽略

​	支持负坐标 

​	Panel.ZIndex 设置显示优先级  大的在上面  默认值为0

```
<Canvas>
	<Button Content="回到顶部" Canvas.Bottom="20" Canvas.Right="20" Height="30" Width="100"></Button>
</Canvas>
```



Grid  网格面板

​	当在同一个单元格中放多个控件时  注意设对齐以及Margin防止控件冲突 

```
ShowGridLines="True"   是否显示边框

//设置宽高度

<RowDefinition Height="40" />
<RowDefinition Height="*"/>
<RowDefinition Height="2*" />
<RowDefinition Height="auto" />

```





<u>带标题的内容控件</u>

普通的容器控件只能放一个元素 Content   此类控件多Header和边框



GroupBox  分组控件

​	内部只能有一个控件

```

```



Expander 可折叠控件 

​	带标题边框 只有一个内容

```
<Expander Grid.Column="1" Width="200" Height="200" Header="可折叠控件" BorderBrush="Red" BorderThickness="2" IsExpanded="True" Padding="10" ExpandDirection="Down" Expanded="Expander_Expanded" Collapsed="Expander_Collapsed">
	<Label Content="这是一个Expander控件"></Label>

</Expander>
```



TabControl 选项卡

​	条目控件

```
TabItem  HeaderContentControl   
TabStripPlacement="Top"  标题栏显示的位置

TabItem tabItem = tab.SelectedItem as TabItem;   //获取tabItem
//object tabitem = tab.SelectedContent;    //获取tabItem中的内容
tabItem.Header = "变变变";

//切换页签的两种方式
tab.SelectedItem = tab.Items[1];
tab.SelectedIndex = 1;
```





Frame   内容控件  支持导航  *可以承载Page页*

​	可以将一个页面导航到另一个页面 

​	在TabItem 中内嵌页面

​	Window是根页面 无法内嵌   

```
NavigationUIVisibility 是否显示导航栏
Source Page资源地址
<Frame Source="TestPage.xaml" NavigationUIVisibility="Visible"></Frame>
```





ListView 

```
<ListView x:Name="listView" Grid.Column="0">
            <ListView.View>
                <GridView>
                    <GridViewColumn Width="80">
                        <GridViewColumn.Header >
                            <CheckBox x:Name="chkAll" Content="全选"></CheckBox>
                        </GridViewColumn.Header>
                        <GridViewColumn.CellTemplate>
                            <DataTemplate>
                                <CheckBox x:Name="chk" Tag="{Binding Id}" IsChecked="{Binding ElementName=chkAll,Path=IsChecked,Mode=OneWay}" HorizontalContentAlignment="Center"></CheckBox>
                            </DataTemplate>
                        </GridViewColumn.CellTemplate>
                    </GridViewColumn>
                    <GridViewColumn Header="编号" DisplayMemberBinding="{Binding Id}" Width="50"></GridViewColumn>
                    <GridViewColumn Header="电影名称" Width="100">
                        <GridViewColumn.CellTemplate>
                            <DataTemplate>
                                <Label Content="{Binding Name}" Foreground="Gray"/>
                            </DataTemplate>
                        </GridViewColumn.CellTemplate>
                    </GridViewColumn>
                    <GridViewColumn Header="发表时间" DisplayMemberBinding="{Binding PublishTime}" Width="100"></GridViewColumn>
                </GridView>
            </ListView.View>
        </ListView>
```



DataGrid

​	派生于 MultiSelector   Selector

​	网格控件 可是在自定义网格中显示数据的控件

```
<DataGrid Grid.Column="1" x:Name="dataGrid" AutoGenerateColumns="False" 
                  HeadersVisibility="Column" AlternatingRowBackground="AliceBlue" 
                  CanUserAddRows="False" IsReadOnly="False" AlternationCount="2"
                  GridLinesVisibility="All" SelectionUnit="Cell" SelectionMode="Single"
                  >
            <DataGrid.RowStyle>
                <Style TargetType="DataGridRow">
                    <Setter Property="Background" Value="Red"></Setter>
                    <Style.Triggers>
                        <Trigger Property="ItemsControl.AlternationIndex" Value="0">
                            <Setter Property="Background" Value="Yellow"></Setter>
                        </Trigger>
                        <Trigger Property="ItemsControl.AlternationIndex" Value="1">
                            <Setter Property="Background" Value="AliceBlue"></Setter>
                        </Trigger>
                    </Style.Triggers>
                </Style>
            </DataGrid.RowStyle>
            <DataGrid.Columns>
                <DataGridTextColumn Header="编号" Binding="{Binding Id}" Width="50"></DataGridTextColumn>
                <DataGridTextColumn Header="电影名称" Binding="{Binding Name}" Width="100">
                    <DataGridTextColumn.HeaderTemplate>
                        <DataTemplate>
                            <TextBlock Foreground="Red" Text="{Binding}"></TextBlock>
                        </DataTemplate>
                    </DataGridTextColumn.HeaderTemplate>
                </DataGridTextColumn>
                <DataGridCheckBoxColumn Header="已观看" Binding="{Binding IsWatched}">
                <!--<DataGridCheckBoxColumn.CellStyle>-->
                </DataGridCheckBoxColumn>
                <DataGridComboBoxColumn x:Name="countryComboBox" Header="国家" SelectedValueBinding="{Binding CountryId}" SelectedValuePath="Id" DisplayMemberPath="Country" Width="100"></DataGridComboBoxColumn>
                <DataGridTemplateColumn Header="发布时间" >
                    <DataGridTemplateColumn.CellTemplate>
                        <DataTemplate>
                            <Label Content="{Binding PublishTime}"></Label>
                        </DataTemplate>
                    </DataGridTemplateColumn.CellTemplate>
                </DataGridTemplateColumn>
            </DataGrid.Columns>
        </DataGrid>
        
        
        
//数据绑定  
//DataGrid
dataGrid.ItemsSource = GetTestList();
//comboBox 取法
//1  x:Name
//countryComboBox.ItemsSource = GetComboBoxTestList();
//2
DataGridComboBoxColumn countryComboBox2 =   dataGrid.Columns[3] as DataGridComboBoxColumn;
countryComboBox2.ItemsSource = GetComboBoxTestList();
```

通过ViewModel 数据绑定 

```
//后端
ViewModel vm = new ViewModel();
vm.DataList = GetTestList();
vm.ComboBoxList = GetComboBoxTestList();
this.DataContext = vm;

//前端
<DataGrid ItemsSource="{Binding DataList}" />


<DataGridComboBoxColumn x:Name="countryComboBox" Header="国家" SelectedValueBinding="{Binding CountryId}" SelectedValuePath="Id" DisplayMemberPath="Country" Width="100">
    <DataGridComboBoxColumn.ElementStyle>
        <Style TargetType="ComboBox">
            <Setter Property="ItemsSource" Value="{Binding DataContext.ComboBoxList,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=Window}}">
            </Setter>	
        </Style>
    </DataGridComboBoxColumn.ElementStyle>
    <DataGridComboBoxColumn.EditingElementStyle>
        <Style TargetType="ComboBox">
            <Setter Property="ItemsSource" Value="{Binding DataContext.ComboBoxList,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=Window}}">
            </Setter>
        </Style>
    </DataGridComboBoxColumn.EditingElementStyle>

</DataGridComboBoxColumn>



```







层次 

​	Solution  解决方案

 	Project

​			三种类型    Winexe (windows应用程序)  Library (类库)   exe(控制台应用程序)

​		properties   资源文件

​		references 引用

​		xaml  声明性语言

```xaml
<Window x:Class="HelloWPF.MainWindow"    //和cs里的class对应 编译后合在一起
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
        //默认名称空间
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        //  加了x 引用其内部标签也要加x  
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWPF"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        
    </Grid>
</Window>
```

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace HelloWPF
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window         //partial  分部类  和xaml对应合并
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
	
```

11.18



Winform 事件驱动  WPF数据驱动



控件分类    F12查看控件类型

​	布局控件  Panel	

​	内容控件  ContentControl   只能容纳一个控件或布局控件

​	带标题内容控件  内容控件可以设置标题Header 父类：HeaderedContentControl

​	条目控件 可以显示一列数据  数据类型一般相同 ItemControl

​		可以设置标题

​	特殊内容控件

​		TextBox PasswordBox TextBlock Image

​	可以引入第三方控件



XAML     构建应用程序用户界面 可扩展应用程序标记语言

命名空间语法 

​	xmlns:[前缀] = “命名空间描述”

​	自定义类或程序集映射语法   xmlns:[必选前缀] = “clr-namespace:[命名空间]；assembly = [程序集名称]”



App.config 配置文件	连接数据库 

App.xaml  系统级资源







Window

​	内容控件里只能承载一个Content

​	ShowInTaskbar="False"   是否在任务栏显示

​	WindowStartupLocation="CenterScreen"  窗体首次显示位置

​	WindowState="Maximized"  窗口显示模式  （最大化最小化）

​	Topmost="True"  显示的优先级  是否在最上层

​	Icon="asset/images/globe.jpg"  图标



RadioButton

```
GroupName="type"   分组
```



CheckBox

```
IsThreeState="True"   存在中间状态   null/true/false
```

Image

```
Source = "";
Stretch = "Stretch/Null/Uniform/UniformToFill" F12看具体属性  默认Uniform
StretchDirection 
```

Border

​	常与布局面板一起使用 

​	只能有一个子元素

​	要显示多个子元素时  内部用一个布局面板作为子元素 布局面板内再存放其他元素

```
BorderBrush="Red"      边框颜色
BorderThickness="2"    边框粗细
CornerRadius="5"       弧度
Background="Yellow"    背景色
Padding
```

下拉框

​	ComboBox 条目控件 ItemControl

​	ComboBoxItem  内容控件

```
IsDropDownOpen="True"   下拉框是否展开

获取数据进行绑定
List<MoviesModel> list = getTestList();
combobox.SelectedValuePath = "Id";
combobox.DisplayMemberPath = "Name";
combobox.Items.Clear();
combobox.ItemsSource = list;  //其他绑定方式参照vs中代码


//事件
SelectionChanged="listBox_SelectionChanged"
//注意获取时类型的转化 存入时什么类型就 as xx

```



列表框  ListBox

```
操作同上 ComboBox

SelectionMode="Multiple"  多选

两个ListBox中的项相互移动时，不适合通过ItemsSource存入  因为涉及到类型的转换 
```

DatePicker

```
自定义格式通过Style,template来实现
```

Calendar

```
DisplayMode="Decade"   首先显示的类型  
SelectionMode="MultipleRange"  选择的模式 要按住Ctrl键
```

Slider 

```
Maximum="90" Minimum="50" Value="51"
Orientation="Horizontal"   方向
TickPlacement="TopLeft"  刻度显示位置
TickFrequency="10"  刻度间隔
IsSnapToTickEnabled="True"  拖动后的值是不是整型
SmallChange="5" LargeChange="10" 拖动小步和大步的位移距离  直接点右边空白处就是一大步
IsDirectionReversed="True"  为true时值方向设置为反向
IsSelectionRangeEnabled="True" SelectionStart="66" SelectionEnd="70"   选范围
```

ProgressBar 进度条

```
Orientation="Vertical"  方向
Minimum="20" Maximum="120"   范围
Value = "30"  完成度    
IsIndeterminate="True"   ture 动态进度   false  显示值

//Dispatcher  线程任务
//线程任务投放
progressBar.Value = 0;
var max = progressBar.Maximum;
    Task.Run(() =>
    {
        for (int i = 0; i < max; i++)
        {
            progressBar.Dispatcher.Invoke(() => {
            progressBar.Value = i;
        });
    	Thread.Sleep(1000);
    }

});


```





<u>布局控件</u>

StackPanel 

​	排列成一行或者一列 可以嵌套

```
Orientation="Vertical"  默认垂直方向排列  
垂直排列时设置水平对齐  水平排列时设置垂直对齐
通过Margin设置边距
FlowDirection="LeftToRight"   水平排列时可设置item布局方向
当子元素过多导致空间不够时,多余子元素会被隐藏
```



WrapPanel 流面板  

​	子元素按顺序排列  如果空间不够 会自动换行/列

```
基本同 StackPanel
ItemHeight="30" ItemWidth="100"  设置子元素宽高
常在StackPanel中嵌套使用
```



DockPanel 停靠面板

​	先添加的子元素优先占有边界  常用于布局自适应页面

```
DockPanel.Dock="Left"  布局位置
LastChildFill="True"  最后元素是否占有所有剩余空间
```

```
<DockPanel LastChildFill="True">
        <StackPanel DockPanel.Dock="Left" Width="20" Background="Red"></StackPanel>
        <StackPanel DockPanel.Dock="Top" Height="20" Background="Yellow"></StackPanel>
        <StackPanel DockPanel.Dock="Right" Width="20" Background="Green"></StackPanel>
        <StackPanel DockPanel.Dock="Bottom" Height="20" Background="Orange"></StackPanel>
        <Grid Background="Gray"></Grid>
    </DockPanel>
```



Canvas  画布面板 坐标面板

​	定义区域，子元素的显示位置，指定相对于面板的坐标

​	两点定位  不用指定上下左右所有的位置  后面的定位会被忽略

​	支持负坐标 

​	Panel.ZIndex 设置显示优先级  大的在上面  默认值为0

```
<Canvas>
	<Button Content="回到顶部" Canvas.Bottom="20" Canvas.Right="20" Height="30" Width="100"></Button>
</Canvas>
```



Grid  网格面板

​	当在同一个单元格中放多个控件时  注意设对齐以及Margin防止控件冲突 

```
ShowGridLines="True"   是否显示边框

共享列宽 

//设置宽高度

<RowDefinition Height="40" />
<RowDefinition Height="*"/>
<RowDefinition Height="2*" />
<RowDefinition Height="auto" />

```



UniformGird 均分

```
<UniformGrid Rows="5" Columns="2">
</UniformGrid>
```







<u>带标题的内容控件</u>

普通的容器控件只能放一个元素 Content   此类控件多Header和边框



GroupBox  分组控件

​	内部只能有一个控件

```

```



Expander 可折叠控件 

​	带标题边框 只有一个内容

```
<Expander Grid.Column="1" Width="200" Height="200" Header="可折叠控件" BorderBrush="Red" BorderThickness="2" IsExpanded="True" Padding="10" ExpandDirection="Down" Expanded="Expander_Expanded" Collapsed="Expander_Collapsed">
	<Label Content="这是一个Expander控件"></Label>

</Expander>
```



TabControl 选项卡

​	条目控件

```
TabItem  HeaderContentControl   
TabStripPlacement="Top"  标题栏显示的位置

TabItem tabItem = tab.SelectedItem as TabItem;   //获取tabItem
//object tabitem = tab.SelectedContent;    //获取tabItem中的内容
tabItem.Header = "变变变";

//切换页签的两种方式
tab.SelectedItem = tab.Items[1];
tab.SelectedIndex = 1;
```





Frame   内容控件  支持导航  *可以承载Page页*

​	可以将一个页面导航到另一个页面 

​	在TabItem 中内嵌页面

​	Window是根页面 无法内嵌   

```
NavigationUIVisibility 是否显示导航栏
Source Page资源地址
<Frame Source="TestPage.xaml" NavigationUIVisibility="Visible"></Frame>
```





ListView 

```
<ListView x:Name="listView" Grid.Column="0">
            <ListView.View>
                <GridView>
                    <GridViewColumn Width="80">
                        <GridViewColumn.Header >
                            <CheckBox x:Name="chkAll" Content="全选"></CheckBox>
                        </GridViewColumn.Header>
                        <GridViewColumn.CellTemplate>
                            <DataTemplate>
                                <CheckBox x:Name="chk" Tag="{Binding Id}" IsChecked="{Binding ElementName=chkAll,Path=IsChecked,Mode=OneWay}" HorizontalContentAlignment="Center"></CheckBox>
                            </DataTemplate>
                        </GridViewColumn.CellTemplate>
                    </GridViewColumn>
                    <GridViewColumn Header="编号" DisplayMemberBinding="{Binding Id}" Width="50"></GridViewColumn>
                    <GridViewColumn Header="电影名称" Width="100">
                        <GridViewColumn.CellTemplate>
                            <DataTemplate>
                                <Label Content="{Binding Name}" Foreground="Gray"/>
                            </DataTemplate>
                        </GridViewColumn.CellTemplate>
                    </GridViewColumn>
                    <GridViewColumn Header="发表时间" DisplayMemberBinding="{Binding PublishTime}" Width="100"></GridViewColumn>
                </GridView>
            </ListView.View>
        </ListView>
```



DataGrid

​	派生于 MultiSelector   Selector

​	网格控件 可是在自定义网格中显示数据的控件

```
<DataGrid Grid.Column="1" x:Name="dataGrid" AutoGenerateColumns="False" 
                  HeadersVisibility="Column" AlternatingRowBackground="AliceBlue" 
                  CanUserAddRows="False" IsReadOnly="False" AlternationCount="2"
                  GridLinesVisibility="All" SelectionUnit="Cell" SelectionMode="Single"
                  >
            <DataGrid.RowStyle>
                <Style TargetType="DataGridRow">
                    <Setter Property="Background" Value="Red"></Setter>
                    <Style.Triggers>
                        <Trigger Property="ItemsControl.AlternationIndex" Value="0">
                            <Setter Property="Background" Value="Yellow"></Setter>
                        </Trigger>
                        <Trigger Property="ItemsControl.AlternationIndex" Value="1">
                            <Setter Property="Background" Value="AliceBlue"></Setter>
                        </Trigger>
                    </Style.Triggers>
                </Style>
            </DataGrid.RowStyle>
            <DataGrid.Columns>
                <DataGridTextColumn Header="编号" Binding="{Binding Id}" Width="50"></DataGridTextColumn>
                <DataGridTextColumn Header="电影名称" Binding="{Binding Name}" Width="100">
                    <DataGridTextColumn.HeaderTemplate>
                        <DataTemplate>
                            <TextBlock Foreground="Red" Text="{Binding}"></TextBlock>
                        </DataTemplate>
                    </DataGridTextColumn.HeaderTemplate>
                </DataGridTextColumn>
                <DataGridCheckBoxColumn Header="已观看" Binding="{Binding IsWatched}">
                <!--<DataGridCheckBoxColumn.CellStyle>-->
                </DataGridCheckBoxColumn>
                <DataGridComboBoxColumn x:Name="countryComboBox" Header="国家" SelectedValueBinding="{Binding CountryId}" SelectedValuePath="Id" DisplayMemberPath="Country" Width="100"></DataGridComboBoxColumn>
                <DataGridTemplateColumn Header="发布时间" >
                    <DataGridTemplateColumn.CellTemplate>
                        <DataTemplate>
                            <Label Content="{Binding PublishTime}"></Label>
                        </DataTemplate>
                    </DataGridTemplateColumn.CellTemplate>
                </DataGridTemplateColumn>
            </DataGrid.Columns>
        </DataGrid>
        
        
        
//数据绑定  
//DataGrid
dataGrid.ItemsSource = GetTestList();
//comboBox 取法
//1  x:Name
//countryComboBox.ItemsSource = GetComboBoxTestList();
//2
DataGridComboBoxColumn countryComboBox2 =   dataGrid.Columns[3] as DataGridComboBoxColumn;
countryComboBox2.ItemsSource = GetComboBoxTestList();
```

通过ViewModel 数据绑定 

```
//后端
ViewModel vm = new ViewModel();
vm.DataList = GetTestList();
vm.ComboBoxList = GetComboBoxTestList();
this.DataContext = vm;

//前端
<DataGrid ItemsSource="{Binding DataList}" />


<DataGridComboBoxColumn x:Name="countryComboBox" Header="国家" SelectedValueBinding="{Binding CountryId}" SelectedValuePath="Id" DisplayMemberPath="Country" Width="100">
    <DataGridComboBoxColumn.ElementStyle>
        <Style TargetType="ComboBox">
            <Setter Property="ItemsSource" Value="{Binding DataContext.ComboBoxList,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=Window}}">
            </Setter>	
        </Style>
    </DataGridComboBoxColumn.ElementStyle>
    <DataGridComboBoxColumn.EditingElementStyle>
        <Style TargetType="ComboBox">
            <Setter Property="ItemsSource" Value="{Binding DataContext.ComboBoxList,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=Window}}">
            </Setter>
        </Style>
    </DataGridComboBoxColumn.EditingElementStyle>

</DataGridComboBoxColumn>



```



Menu :MenuBase  条目控件

```
Menu
	IsMainMenu     alt或F10 能否激活
MenuItem   
	InputGestureText   快捷键配置
```

ContextMenu   右键菜单 建立在目标元素上

```
<Label x:Name="lbl" Grid.Column="1" Content="右键菜单" HorizontalAlignment="Left" VerticalAlignment="Top" Background="Gray" MouseLeftButtonDown="lbl_MouseLeftButtonDown" ContextMenuService.Placement ="Top" >
<Label.ContextMenu  >
	<ContextMenu x:Name="lblMenu" IsOpen="False" HorizontalOffset="10" VerticalOffset="10" HasDropShadow="False">
        <MenuItem Header="菜单1"/>
        <MenuItem Header="菜单2"/>
	</ContextMenu>
</Label.ContextMenu>
</Label>
```



​	ToolBar    -> HeaderedItemsControl   带标题条目控件

​		工具栏 容器

```
Orientation 只读
ToolBarTray 内部放多个ToolBar
在ToolBarTray中 为ToolBar设置Band表示所在行  BandIndex表示当前行顺序(带区索引)
IsLocked="True"   内部ToolBar是否可拖动
```



StatusBar     

​	StatusItem ->       ContentControl   

​		状态栏 显示一些信息      版权/用户信息/时间/当前模块/进度条

​		一般显示在底部   类似软件显示： 当前所在行列 

```

```



MediaElement 

​	mp4无法打开时   其属性需要修改：生成操作：内容    复制到输出：如果较新则复制







