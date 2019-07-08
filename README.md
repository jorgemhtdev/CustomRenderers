# Custom Renderers - Examples
 
### Introduction 

![](https://img.shields.io/teamcity/codebetter/bt428.svg) [![License:MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
# Custom hamburger icon MasterDetailPage
### iOS
```
[assembly: ExportRenderer(typeof(MasterDetailPage), typeof(IconMasterDetailPageRenderer))]
namespace YOURNAMESPACE.iOS.Renderers
{
    public class IconMasterDetailPageRenderer : PhoneMasterDetailRenderer
    {
        public override void ViewDidLayoutSubviews()
        {
            base.ViewDidLayoutSubviews();
            if (!(Element is MasterDetailPage mdp)) return;
            if (!(Platform.GetRenderer(mdp.Detail) is UINavigationController nc)) return;
    
            var btn = new UIButton(UIButtonType.Custom);
            var img = UIImage.FromFile("BurgerMenu");
            btn.SetImage(img, UIControlState.Normal);
            btn.TouchUpInside += (sender, e) => mdp.IsPresented = true;
    
            var lbbi = new UIBarButtonItem(btn);
            nc.NavigationBar.TopItem.LeftBarButtonItem = lbbi;
        }
    }
}
```
Source: [Xamarin.Forms official home](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/PhoneMasterDetailRenderer.cs)
### Android
```
[assembly: ExportRenderer(typeof(MasterDetailPage), typeof(IconMasterDetailPageRenderer))]
namespace Goclubbing.Droid.Renderers
{
    public class IconMasterDetailPageRenderer : MasterDetailPageRenderer
    {
        public IconMasterDetailPageRenderer(Context context) : base(context)
        {
        }

        private static Android.Support.V7.Widget.Toolbar GetToolbar() =>
            (CrossCurrentActivity.Current?.Activity as MainActivity)?.FindViewById<Android.Support.V7.Widget.Toolbar>(
                Resource.Id.toolbar);

        protected override void OnLayout(bool changed, int l, int t, int r, int b)
        {
            base.OnLayout(changed, l, t, r, b);
            var toolbar = GetToolbar();
            if (toolbar != null)
            {
                for (var i = 0; i < toolbar.ChildCount; i++)
                {
                    var imageButton = toolbar.GetChildAt(i) as ImageButton;

                    if (!(imageButton?.Drawable is DrawerArrowDrawable drawerArrow))
                        continue;

                    imageButton.SetImageDrawable(Context.GetDrawable(Resource.Drawable.BurgerMenu));
                }
            }
        }
    }
}
```
Source: [Xamarin.Forms official home](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/AppCompat/MasterDetailPageRenderer.cs)
# Button with Padding
### PCL
```
public class EnhancedButton : Button
{
    #region Padding    

    public static BindableProperty PaddingProperty = BindableProperty.Create(nameof(Padding), typeof(Thickness), typeof(EnhancedButton), default(Thickness), defaultBindingMode:BindingMode.OneWay);

    public Thickness Padding
    {
        get { return (Thickness) GetValue(PaddingProperty); }
        set { SetValue(PaddingProperty, value); }
    }

    #endregion Padding
}
```
### iOS
```
[assembly: ExportRenderer(typeof(EnhancedButton), typeof(EnhancedButtonRenderer))]
namespace YOURNAMESPACE.iOS
{
    public class EnhancedButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            base.OnElementChanged(e);
            UpdatePadding();
        }

        private void UpdatePadding()
        {
            var element = this.Element as EnhancedButton;
            if (element != null)
            {
                this.Control.ContentEdgeInsets = new UIEdgeInsets(

                    (int)element.Padding.Top,
                    (int)element.Padding.Left,
                    (int)element.Padding.Bottom,
                    (int)element.Padding.Right
                );
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            base.OnElementPropertyChanged(sender, e);
            if (e.PropertyName == nameof(EnhancedButton.Padding))
            {
                UpdatePadding();
            }
        }
    }
}
```
### Android
```
[assembly: ExportRenderer(typeof(EnhancedButton), typeof(EnhancedButtonRenderer))]
namespace YOURNAMESPACE.Droid
{
    public class EnhancedButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            base.OnElementChanged(e);
            UpdatePadding();
        }

        private void UpdatePadding()
        {
            var element = this.Element as EnhancedButton;
            if (element != null)
            {
                this.Control.SetPadding(
                    (int)element.Padding.Left,
                    (int)element.Padding.Top,
                    (int)element.Padding.Right, 
                    (int)element.Padding.Bottom
                );
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            base.OnElementPropertyChanged(sender, e);
            if (e.PropertyName == nameof(EnhancedButton.Padding))
            {
                UpdatePadding();
            }
        }
    }
}
```
### Usage
```
    <customControls:EnhancedButton
        Padding="10,10"
        Text="Buttom Padding" />
```
Source: [Stackoverflow](https://stackoverflow.com/questions/30270979/xamarin-forms-how-can-i-add-padding-to-a-button)
