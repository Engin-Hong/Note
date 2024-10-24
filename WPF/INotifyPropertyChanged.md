# 属性需要被直接绑定，该属性类型对应的PropertyChanged才不为null
## Example
- XAML代码
```xml
    <TextBox Text="{Binding TestEntry.Self}"/>
```

- XAML后台代码
<para>
```csharp
    VM = new ViewModel();
    this.DataContext = VM;
```
</para>


- ViewModel代码
```csharp
    public class ViewModel
    {
        public Entry TestEntry { get; set; }
        public ViewModel()
        {
            TestEntry=new Entry(){ Self = new JProperty("Empty",new JValue(22))};
        }
    }
```

- Entry类代码
```csharp
 public class Entry : IEntry,INotifyPropertyChanged
 {
     public event PropertyChangedEventHandler PropertyChanged;
     protected void OnPropertyChanged([CallerMemberName] string name = null)
     {
         if(PropertyChanged != null)
         {
             PropertyChanged.Invoke(this,new PropertyChangedEventArgs(name));
         }
     }
     public object Self {  get; set; } 
     public override string ToString() => Self.ToString();
 }
```
- 直接被绑定的属性需要实现INotifyPropertyChanged接口才能在后台改变相应属性时,前台得到通知
- 属性所属实例的父实例(此处为ViewModel)不必实现INotifyPropertyChanged接口