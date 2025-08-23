# Keyboard handling on Xamarin Forms

## Keyboard Custom Renderer (Usecase : Margin customisation according to Keyboard Show/Hide Event)
This example will see how the keyboard event can be handled on ios and android via Custom Renderer in Xamarin Forms.
> ref: 
> https://riptutorial.com/xamarin-forms/example/10022/custom-renderer-for-listview
> https://forums.xamarin.com/discussion/67333/set-selected-list-view-background-color 
> https://stackoverflow.com/questions/2833057/background-listview-becomes-black-when-scrolling

### Common Part
Main View.xaml
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             xmlns:extensions="clr-namespace:[YourNamespace];assembly=[proj name]">
    <ContentPage.Content>
      <extensions:ExtendedKeyboard>
        ...
      </extensions:ExtendedKeyboard>
      
      ...
      ...
```
> [YourNamespace] = [Proj name].[folder]

New Custom controler (Renderer)
```xaml
using Xamarin.Forms;

namespace [Proj name].[folder]
{
    public class ExtendedKeyboard : Grid
    {
    }
}
```
> put all the elements in a container view (e.g. Grid) and only change the margin of this view

### IOS Extended Renderer


```xaml
...
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ExtendedKeyboard), typeof(ExtendedKeyboardRenderer))]
namespace [proj name].iOS.Renderers
{
    public class ExtendedKeyboardRenderer : ViewRenderer
    {
        private NSObject _showObserver;
        private NSObject _hideObserver;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            AnimationsEnabled = false;
            if (e.NewElement != null)
            {
                _showObserver = _showObserver ?? UIKeyboard.Notifications.ObserveWillShow(OnKeyboardShow);
                _hideObserver = _hideObserver ?? UIKeyboard.Notifications.ObserveWillHide(OnKeyboardHide);
            }
        }

        private void OnKeyboardShow(object sender, UIKeyboardEventArgs args)
        {
            if (args.Notification.UserInfo == null) return;
            
            var result =
                (NSValue) args.Notification.UserInfo.ObjectForKey(new NSString(UIKeyboard.FrameEndUserInfoKey));
            CGSize keyboardSize = result.RectangleFValue.Size;

            if (Element != null)
            {
                Element.Margin = new Thickness(0, 0, 0, keyboardSize.Height);
            }
        }
        
        private void OnKeyboardHide(object sender, UIKeyboardEventArgs args)
        {
            if (Element != null)
            {
                Element.Margin = new Thickness(0);
            }
        }
    }
}
```

### Android Extended Renderer
Android does not notify the user with those events as reliable as iOS does.
For this example, we need to use an special way Android has

MainActivity.cs
```c#
...
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;

namespace [proj name].Droid
{
  ...
  public class MainActivity : ...
  {
      protected override void OnCreate(Bundle savedInstanceState)
      {
        ...
        App.Current
          .On<Xamarin.Forms.PlatformConfiguration.Android>()
          .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
          
        ...
}
```
> if project is not mvx, you can place the App.current... line into the below of App().
