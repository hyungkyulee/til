# MvvmCross Messenger Service on Xamarin Forms

- define a message model
```
using MvvmCross.Plugin.Messenger;

namespace [project name].Messages
{
    public class RefreshBarMenuMessage : MvxMessage
    {
        public RefreshBarMenuMessage(object sender): base(sender)
        {
        }
    }
}
```

- [View] define a messenger instance and token to subscribe the message model
  and a message handler
```
public partial class xxxx
{
  private IMvxMessenger _messenger;
  private MvxSubscriptionToken _token;

  ...
  public xxx()
  {
    InitializeComponent();
    _messenger = Mvx.IoCProvider.Resolve<IMvxMessenger>();   
    _token = _messenger.SubscribeOnMainThread<RefreshBarMenuMessage>(OnRefreshBarMenu);
    ...
  }
  
  private void OnRefreshBarMenu(RefreshBarMenuMessage obj)
  {
    // handle the message
  }
  
```

- [ViewModel] create a IMvxMessenger and initialise
> it can be fired from any ViewModel, differently with the View - ViewModel PropertyChanged Event

```
using MvvmCross.Plugin.Messenger;

...

public partial class xxxx
{
  private readonly IMvxMessenger _messenger;

  ...

  public xxx(IMvxMessenger messenger)
  {
    _mvxMessenger = messenger;
    ...
    
  }
```
> a messenger object service can be assigned in a constructor with a singleton to use it as a global variable

- [ViewModel] fire a broadcasting service with the messenger in a event method.
```
private void OnClickedButton (bool mode)
{
  _mvxMessenger.Publish(new RefreshBarMenuMessage(this));
  ...
```
