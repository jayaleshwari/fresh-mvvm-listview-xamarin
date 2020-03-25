# How to bind data using FreshMVVM in Xamarin.Forms ListView (SfListView)?

The Xamarin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview?) allows you to work with [FreshMVVM](https://github.com/rid00z/FreshMvvm) framework. Please follow the below steps to work with FreshMVVM,

**Step 1:** Install the FreshMvvm NuGet package in the shared code project. 

**Step 2:** Create your XAML page (view) with name ending with “Page”.
```c#
namespace ListViewXamarin
{
    public partial class ListViewPage : ContentPage
    {
        public ListViewPage()
        {
            InitializeComponent();
        }
    }
}
```
**Step 3:** Create a page model with the name ending with PageModel and inherit [FreshBasePageModel](https://github.com/rid00z/FreshMvvm/blob/master/src/FreshMvvm/FreshBasePageModel.cs). If your Page name is MainPage, then the PageModel name should be MainPageModel and the namespace of Page and PageModel should be the same. In this PageModel, you can keep the ViewModel related properties.
``` c#
using FreshMvvm;
public class ListViewPageModel : FreshBasePageModel
{
    public ObservableCollection<Contacts> contactsinfo { get; set; }
 
    public ListViewPageModel()
    {
        contactsinfo = new ObservableCollection<Contacts>();
        GenerateInfo();
    }
        
    public void GenerateInfo()
    {
        Random r = new Random();
        for (int i = 0; i < 30; i++)
        {
            var contact = new Contacts(CustomerNames[i], r.Next(720, 799).ToString() + " - " + r.Next(3010, 3999).ToString());
            contact.ContactImage = ImageSource.FromResource("ListViewXamarin.Images.Image" + r.Next(0, 28) + ".png");
            contactsinfo.Add(contact);
        }
    }
}
```
**Step 4:** Create model class which inherits FreshBasePageModel and use RaisePropertyChanged method of base class to raise property changed notifier.
``` c#
namespace ListViewXamarin
{
    public class Contacts : FreshBasePageModel
    {
        private string contactName;
 
        public Contacts()
        {
 
        }
 
        public string ContactName
        {
            get { return contactName; }
            set
            {
                if (contactName != value)
                {
                    contactName = value;
                    this.RaisePropertyChanged();
                }
            }
        }
    }
}
```
**Step 5:** Set MainPage using PageModel in your App.xaml.cs file.
``` C#
public App()
{
    InitializeComponent();
 
    var page = FreshPageModelResolver.ResolvePageModel<ListViewPageModel>();
    var basicNavContainer = new FreshNavigationContainer(page);
    MainPage = basicNavContainer;
}
```
**Step 6:** Bind the viewmodel collection to the [SfListView.ItemSource](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~ItemsSource.html?) property.
``` xml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewXamarin"
             xmlns:syncfusion="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms"
             x:Class="ListViewXamarin.ListViewPage">
    <ContentPage.Content>
        <StackLayout>
            <syncfusion:SfListView x:Name="listView"
                                   ItemSpacing="1" 
                                   ItemSize="60"
                                   ItemsSource="{Binding contactsinfo}">
                <syncfusion:SfListView.ItemTemplate >
                    <DataTemplate>
                        <Label LineBreakMode="NoWrap"
                             TextColor="#474747"
                             Text="{Binding ContactName}"/>
                    </DataTemplate>
                </syncfusion:SfListView.ItemTemplate>
            </syncfusion:SfListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
