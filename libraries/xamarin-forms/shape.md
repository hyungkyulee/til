# Shapes

## Round Image and Label under the image
```
<Frame BackgroundColor="{StaticResource xxxBlue}" HasShadow="False" Margin="0, 0, 0, 5" Padding="0"
       CornerRadius="30" IsClippedToBounds="True" HeightRequest="60" WidthRequest="60"
       HorizontalOptions="Center"
       VerticalOptions="Start">
    <Image
        x:Name="IconLocation"
        HorizontalOptions="Center"
        VerticalOptions="Center"
        WidthRequest="30"
        HeightRequest="30">

        <Image.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnClick_Location" />
        </Image.GestureRecognizers>
    </Image>
</Frame>
<Label Text="{Binding SelectedRegion.Name}" TextColor="{StaticResource xxxBlue}" HorizontalOptions="CenterAndExpand" />
 
```
