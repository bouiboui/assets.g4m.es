## What is MagicList
MagicList is a Unity component that allows you to easily create vertical or horizontal lists that update automatically with your data.
It provides handy menu items to create pre-configured vertical or horizontal lists, saving you a lot of time.

It does the minimum amount of operations to remove, add or update the items that changed instead of clearing the list and re-instantiating all the children which can be very bad performance-wise.

## How to Start
- Right-click on your scene Hierarchy and select `UI/MagicList/Vertical` (or `Horizontal`)
- Read [the tutorial](./TUTORIAL.md) to learn how to use it with a step-by-step example
- Unpack `Example.unitypackage` to see what the finished tutorial looks like

> **New! Interactivity Guide:**  
> If you plan to use MagicList for interactive list items (for instance, to select, buy, or sell items), be sure to check out [INTERACT.md](./INTERACT.md) for a complete guide on how to implement item interactions.

## FAQ

> Can I use MagicList with a `List<>` variable?

- No, you have to change it into an `ObservableCollection` which has roughly the same interface and allows MagicList to be a lot more performant.

> MagicList.SetData was never called
- This error means that you didn't link your data to your MagicList.
     
    - Create a MonoBehaviour Script with a public `magicList` property like this: 
    
    ```csharp
    public MagicList.Scripts.MagicList magicList;
    ```
    
    - Override the Awake method to call "magicList.SetData()" like this:
    
    ```csharp
    private void Awake() { 
      magicList.SetData( YOUR_DATA_VARIABLE ); 
    }
    ```
    
    - Drag `Vertical List` or `Horizontal List` from your Hierarchy to the `magicList` property of your component

For any other question, if you don't find the solution in the tutorial feel free to reach me from the Asset Store :)
