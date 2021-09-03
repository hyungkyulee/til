# CollectionView vs ListView

## CollectionView
> ref: https://medium.com/a-developer-in-making/how-to-work-with-collectionview-in-xamarin-forms-5dc65c50b419

### Selection Action
#### by TabGesture
```
<CollectionView x:Name="GroupFilterList" 
            x:DataType="viewModels:GroupViewModel"
            ItemsSource="{Binding AvailableGroup}"
            ItemsUpdatingScrollMode="KeepItemsInView"
            Style="{StaticResource settingsCollectionView}">
<CollectionView.ItemTemplate>
    <DataTemplate>
        <Frame x:DataType="models:FilterOption"
               Style="{StaticResource baseDataItemFrame}">
            <Frame.GestureRecognizers>
                <TapGestureRecognizer
                  Command="{Binding
                    Source={RelativeSource AncestorType={x:Type viewModels:GroupViewModel}},
                    Path=SelectGroupCommand}"
                  CommandParameter="{Binding .}"/>
            </Frame.GestureRecognizers>
            <Grid ColumnSpacing="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width=".85*" />
                    <ColumnDefinition Width=".15*" />
                </Grid.ColumnDefinitions>
```

#### by SelectionChanged Event
```
<CollectionView x:Name="GroupFilterList" 
            x:DataType="viewModels:GroupViewModel"
            ItemsSource="{Binding AvailableGroup}"
            ItemsUpdatingScrollMode="KeepItemsInView"
            SelectionMode="Single"
            SelectionChanged="GroupFilterList_OnSelectionChanged"
            Style="{StaticResource settingsCollectionView}">
<CollectionView.ItemTemplate>
    <DataTemplate>
        <Frame x:DataType="models:FilterOption"
               Style="{StaticResource baseDataItemFrame}">
            <Grid ColumnSpacing="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width=".85*" />
                    <ColumnDefinition Width=".15*" />
                </Grid.ColumnDefinitions>
```
