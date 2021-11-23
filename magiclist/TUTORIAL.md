Tutorial
--------

**1) Add a Vertical MagicList to your scene**
- Right-click in the Hierarchy and select `UI/MagicList/Vertical`
- In the Inspector, find the `MagicList` component and click on the `Generate List Item Prefab` button

**2) Create a rendering script for your list items**
- Double-click on the `ListItemPrefab` property of the `MagicList` component to open the prefab
- In the Inspector, click on `Add component`, type `MyListItemRenderer`, and create a new Script (click on `Create and Add`)
- Once created, double-click on the greyed out `MyListItemRenderer` Script to open it with your code editor
- Replace all its contents with the following code and save:

```csharp
using MagicList.Scripts;
using UnityEngine;
using UnityEngine.UI;

public class MyListItemRenderer : MonoBehaviour, IListItemRenderer
{
   private Text _itemText;

   private void Awake()
   {
       _itemText = transform.Find(`Text`).GetComponent<Text>();
   }

   public void UpdateView(object value)
   {
       _itemText.text = (string) value;
   }
}
```
   
- Remove `ListItemPrefab` from the scene Hierarchy (not from your Assets!)
   
**3) Create and bind your data**
- In the Inspector, click on `Add Component`, type `MyDataSource` and create a new Script (click on `Create and Add`)
- Once created, double-click on `MyDataSource` to open your code editor
- Replace all its contents with the following code and save:

```csharp
using System.Collections;
using System.Collections.ObjectModel;
using UnityEngine;

public class MyDataSource : MonoBehaviour
{
   private readonly ObservableCollection<string> _data = new();
   public MagicList.Scripts.MagicList magicList;

   private void Awake()
   {
       magicList.SetData(_data);
       StartCoroutine(RandomizeData());
   }

   private IEnumerator RandomizeData()
   {
       for (var i = 0; i < Random.Range(0, _data.Count); i++)
           _data.RemoveAt(i);

       for (var i = 0; i < Random.Range(0, 10); i++)
           _data.Add(Random.Range(1, 1000).ToString());

       yield return new WaitForSeconds(.1f);
       yield return RandomizeData();
   }
}
```

- Drag `Vertical List` from your Hierarchy to the `Magic List` property of your component
- Press Play!
   
As you can see, the data source is updated every 1/10th of a second and the Scroll View list is updated automatically.

The scrollbar remains usable, it doesn't jump to the start at every update.  
