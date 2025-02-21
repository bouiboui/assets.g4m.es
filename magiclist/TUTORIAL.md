# MagicList Tutorial

This tutorial will guide you through creating a dynamic list with buttons using MagicList.

## 1. Add a Vertical MagicList to your scene

- Right-click in the Hierarchy and select `UI/MagicList/Vertical`.
- In the Inspector, find the `MagicList` component and click on the `Generate` button in the `List Item Prefab` section.

## 2. Configure the List Item Prefab

- Double-click on the `ListItemPrefab` property of the `MagicList` component to open the prefab.
- In the Hierarchy, you should only see a `Text` GameObject.
- Right-click on `ListItemPrefab` above and add a Button by choosing `UI/Legacy/Button`.
- Rename the Button "BuyButton".
- Now the hierarchy should look like this:
```
ListItemPrefab
- Text
- BuyButton
```
- Change the button's Text to "Buy".

## 3. Create a rendering script for your list items

- In the Hierarchy, select the `ListItemPrefab`.
- In the Inspector, click on `Add component`, type `MyListItemRenderer`, and create a new Script (press `<Enter>` twice).
- Once created, double-click on the greyed out `MyListItemRenderer` script to open it with your code editor.
- Replace all its contents with the following code and save:

```csharp
using UnityEngine;
using UnityEngine.UI;

public class MyListItemRenderer: MonoBehaviour, IListItemRenderer<MyData>
{
    private Button _itemBuyButton;
    private Text _itemText;
    private MyData _currentData;

    private void Awake()
    {
        _itemText = transform.Find("Text").GetComponent<Text>();
        _itemBuyButton = transform.Find("BuyButton").GetComponent<Button>();
    }

    public void BindView(MyData data)
    {
        _currentData = data;
        _itemText.text = data.Name;

        _itemBuyButton.onClick.RemoveAllListeners();
        _itemBuyButton.onClick.AddListener(OnBuyButtonClicked);
    }

    private void OnBuyButtonClicked()
    {
        Debug.Log($"Spending {_currentData.Value} to buy item {_currentData.Name}");
    }
}
```
   
- Go back to the main Scene view (quit the Prefab view by clicking on `<`) 
- Remove `List Item` from the scene Hierarchy (should be under `Canvas/Vertical List/Viewport/Content`)
   
## 4. Create and bind your data

- In the Hierarchy, select `Canvas/Vertical List`.
- In the Inspector, click on `Add Component`, type `MyDataSource` and create a new Script (press `<Enter>` twice).
- Once created, double-click on `MyDataSource` to open your code editor.
- Replace all its contents with the following code and save:

```csharp
using System.Collections.ObjectModel;
using UnityEngine;

public class MyData
{
    public string Name;
    public int Value;

    public MyData(string name, int value)
    {
        Name = name;
        Value = value;
    }
}

public class MyDataSource: MonoBehaviour
{
    private readonly ObservableCollection<MyData> _data = new()
    {
        new MyData("Obj 1", 10),
        new MyData("Obj 2", 20),
        new MyData("Obj 3", 30)
    };

    public MagicList magicList;

    private void Awake()
    {
        magicList.SetData(_data);
    }
}
```

- Drag the `Vertical List` GameObject from the Hierarchy to the `Magic List` property of your `MyDataSource` component in the Inspector.
- Press Play!
