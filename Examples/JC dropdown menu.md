# What is a Dropdown Menu in Android?
#### A dropdown menu displays multiple choices. Users can tap on any one of them. We display the menu when the user interacts with a UI element (like an icon or button) or performs a specific action.

#### Jetpack Compose provides 2 dropdown menu APIs:
- Dropdown Menu
- Exposed Dropdown Menu
----

**1. Dropdown Menu**

This is a normal menu.

Example:

![Jetpack-compose-dropdown-menu-items](/storage/emulated/0/Documents/markor/Jetpack-compose-dropdown-menu-items.png)


The drop down menu uses the position of the parent layout to position itself on the screen. Usually, we place the drop down menu in a box with a sibling (like a button) that will be used as the ‘anchor’. Whenever we tap on this anchor, the menu will be displayed.

Let’s make the above menu.

First, create a MyUI() composable outside the MainActivity class. We will write our code in it.

```kotlin
@Composable
fun MyUI() {

}
```

Call it inside the onCreate() method like this:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            YourProjectNameTheme(darkTheme = false) {
                Column(
                    modifier = Modifier
                        .fillMaxSize()
                        .background(color = Color.White),
                    verticalArrangement = Arrangement.Center,
                    horizontalAlignment = Alignment.CenterHorizontally
                ) {
                    MyUI()
                }
            }
        }
    }
}

@Composable
fun MyUI() {

}
```

Create the items list using the Kotlin array function.

```kotlin
val listItems = arrayOf("Favorites", "Options", "Settings", "Share")
```

The second item is disabled in the above menu. So, create a variable and store its index value. Also, we will display the Toast when the user taps on the menu item. We need a context for that.

```kotlin 
@Composable
fun MyUI() {
    val listItems = arrayOf("Favorites", "Options", "Settings", "Share")
    val disabledItem = 1
    val contextForToast = LocalContext.current.applicationContext
}
```


The menu should be displayed when we tap on the icon. Create an IconButton() and put it inside a Box.


```kotlin
Box(
    contentAlignment = Alignment.Center
) {

    IconButton(onClick = {
        // we will open the menu here
    }) {
        Icon(
            imageVector = Icons.Default.MoreVert,
            contentDescription = "Open Options"
        )
    }
}
```

Let’s look at the DropdownMenu() API.


```kotlin
@Composable
fun DropdownMenu(
    expanded: Boolean,
    onDismissRequest: () -> Unit,
    modifier: Modifier = Modifier,
    offset: DpOffset = DpOffset(0.dp, 0.dp),
    properties: PopupProperties = PopupProperties(focusable = true),
    content: @Composable ColumnScope.() -> Unit
)
```

expanded – Whether the menu is currently open and visible to the user.

onDismissRequest – Called when the user requests to dismiss the menu, such as by tapping on the screen outside the menu’s bounds or when the back key is pressed.

modifier – The modifier.

offset – It is used to control the position of the menu on the screen.

properties – Used to customize the behavior of the menu.

content – The content of the menu. It typically includes DropdownMenuItems as well as custom content. Note that the content is placed inside a scrollable Column. So, If you use a LazyColumn as the root layout, you will end up getting an error.

Create a variable to handle the state of the menu.


```kotlin
// state of the menu
var expanded by remember {
    mutableStateOf(false)
}

Box(
    contentAlignment = Alignment.Center
) {

    // options button
    IconButton(onClick = {
        expanded = true
    }) {
        Icon(
            imageVector = Icons.Default.MoreVert,
            contentDescription = "Open Options"
        )
    }

    // drop down menu
    DropdownMenu(
        expanded = expanded,
        onDismissRequest = {
            expanded = false
        }
    ) {

    }
}
```

We need to add the menu items using the DropdownMenuItem() composable. Here is how it looks:
```kotlin
@Composable
fun DropdownMenuItem(
    onClick: () -> Unit,
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    contentPadding: PaddingValues = MenuDefaults.DropdownMenuItemContentPadding,
    interactionSource: MutableInteractionSource = remember { MutableInteractionSource() },
    content: @Composable RowScope.() -> Unit
)
```

onClick – Called when the item was clicked.

enabled – If the item is enabled. If it is false, the item will not be clickable, and onClick will not be invoked.

contentPadding – The padding applied to the content of this menu item.

interactionSource – The MutableInteractionSource.

content – The content of the menu item. It is a row scope. So, if you add multiple composables, they are placed horizontally.

Since our listItems is of type Array, we can use Kotlin’s forEachIndexed() to place them inside the menu.


```kotlin
listItems.forEachIndexed { itemIndex, itemValue ->
    DropdownMenuItem(
        onClick = {
            Toast.makeText(contextForToast, itemValue, Toast.LENGTH_SHORT)
                .show()
            expanded = false
        },
        enabled = (itemIndex != disabledItem)
    ) {
        Text(text = itemValue)
    }
}
```

Our final code looks like this:


```kotlin
@Composable
fun MyUI() {
    val listItems = arrayOf("Favorites", "Options", "Settings", "Share")
    val disabledItem = 1
    val contextForToast = LocalContext.current.applicationContext

    // state of the menu
    var expanded by remember {
        mutableStateOf(false)
    }

    Box(
        contentAlignment = Alignment.Center
    ) {

        // options button
        IconButton(onClick = {
            expanded = true
        }) {
            Icon(
                imageVector = Icons.Default.MoreVert,
                contentDescription = "Open Options"
            )
        }

        // drop down menu
        DropdownMenu(
            expanded = expanded,
            onDismissRequest = {
                expanded = false
            }
        ) {
            // adding items
            listItems.forEachIndexed { itemIndex, itemValue ->
                DropdownMenuItem(
                    onClick = {
                        Toast.makeText(contextForToast, itemValue, Toast.LENGTH_SHORT)
                            .show()
                        expanded = false
                    },
                    enabled = (itemIndex != disabledItem)
                ) {
                    Text(text = itemValue)
                }
            }
        }
    }
}
```




