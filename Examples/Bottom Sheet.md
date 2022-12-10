### How to create a Bottom Sheet with Jetpack Compose

In Jetpack Compose, we have two types of bottom sheets, the  <Span style="background-color: #DCDCDC"> BottomSheetScaffold</span>   and the  <Span style="background-color: #DCDCDC"> ModalBottomSheetLayout</span>  .



-  <Span style="background-color: #DCDCDC"> BottomSheetScaffold</span>   doesn’t block the screen’s main UI when it appears, and you can view and interact with both (main UI and bottom sheet) simultaneously. Also, you can set a peak height to show part of the bottom sheet all the time.

-  <Span style="background-color: #DCDCDC">ModalBottomSheetLayout </span>   acts like a popup view. It shows up when you press a button and blocks any interaction with the rest of the screen.


###  Contents
1. Creating the Top Bar
2. Creating Bottom Sheet’s content
2.1. Creating the Column Item
2.2. Creating the Column
3. Implementing the Bottom Sheet
3.1. BottomSheetScaffold
3.2. ModalBottomSheetLayout

#### Creating the Top Bar
Go to your compose activity (e.g., MainActivity.kt), and outside the class, add the following composable function to create a Top Bar


```Kotlin
@Composable
fun TopBar() {
    TopAppBar(
        title = { Text(text = stringResource(R.string.app_name), fontSize = 18.sp) },
        backgroundColor = colorResource(id = R.color.colorPrimary),
        contentColor = Color.White
    )
}

@Preview(showBackground = true)
@Composable
fun TopBarPreview() {
    TopBar()
}
```


#### Creating Bottom Sheet’s content

To be more organized, I’m adding all the code about bottom sheet content to a different file named BotttomSheetContentScreen.kt

Inside the BotttomSheetContentScreen, we define a composable function called BottomSheetListItem, and the parameters icon, title, and onItemClick


```Kotlin
@Composable
fun BottomSheetListItem(icon: Int, title: String, onItemClick: (String) -> Unit) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .clickable(onClick = { onItemClick(title) })
            .height(55.dp)
            .background(color = colorResource(id = R.color.colorPrimary))
            .padding(start = 15.dp), verticalAlignment = Alignment.CenterVertically
    ) {
        Icon(painter = painterResource(id = icon), contentDescription = "Share", tint = Color.White)
        Spacer(modifier = Modifier.width(20.dp))
        Text(text = title, color = Color.White)
    }
}

@Preview(showBackground = true)
@Composable
fun BottomSheetListItemPreview() {
    BottomSheetListItem(icon = R.drawable.ic_share, title = "Share", onItemClick = { })
}
```



#### Creating the Column

Now that we have created the item, it’s time to make the column.

In a new compose function called BottomSheetContent, we add the following:



```Kotlin
@Composable
fun BottomSheetContent() {
    val context = LocalContext.current
    Column {
        BottomSheetListItem(
            icon = R.drawable.ic_share,
            title = "Share",
            onItemClick = { title ->
                Toast.makeText(
                    context,
                    title,
                    Toast.LENGTH_SHORT
                ).show()
            })
        BottomSheetListItem(
            icon = R.drawable.ic_link,
            title = "Get link",
            onItemClick = { title ->
                Toast.makeText(
                    context,
                    title,
                    Toast.LENGTH_SHORT
                ).show()
            })
        BottomSheetListItem(
            icon = R.drawable.ic_edit,
            title = "Edit name",
            onItemClick = { title ->
                Toast.makeText(
                    context,
                    title,
                    Toast.LENGTH_SHORT
                ).show()
            })
        BottomSheetListItem(
            icon = R.drawable.ic_delete,
            title = "Delete collection",
            onItemClick = { title ->
                Toast.makeText(
                    context,
                    title,
                    Toast.LENGTH_SHORT
                ).show()
            })
    }
}

@Preview(showBackground = true)
@Composable
fun BottomSheetContentPreview() {
    BottomSheetContent()
}

```


#### Implementing the Bottom Sheet


#### BottomSheetScaffold
In the Compose Activity (e.g., MainActivity.kt), we’re going to add the BottomSheetScaffold composable function and create the variables bottomSheetScaffoldState and scope that we’ll use in our MainScreen to expand/collapse the bottom sheet when we press a button.


```Kotlin
class MainActivity : ComponentActivity() {

    @OptIn(ExperimentalMaterialApi::class)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val bottomSheetScaffoldState = rememberBottomSheetScaffoldState()
            val scope = rememberCoroutineScope()
            BottomSheetScaffold(
                sheetContent = {
                    /* Add code later */
                },
                scaffoldState = bottomSheetScaffoldState,
            ) {
                /* Add code later */
            }
        }
    }
}

@OptIn(ExperimentalMaterialApi::class)
@Composable
fun MainScreen(scope: CoroutineScope, state: BottomSheetScaffoldState) {
    Column(
        Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Button(
            colors = ButtonDefaults.buttonColors(
                backgroundColor = colorResource(id = R.color.colorPrimary),
                contentColor = Color.White
            ),
            onClick = {
                scope.launch {
                    if (state.bottomSheetState.isCollapsed) {
                        state.bottomSheetState.expand()
                    } else {
                        state.bottomSheetState.collapse()
                    }
                }
            }) {
            if (state.bottomSheetState.isCollapsed) {
                Text(text = "Open Bottom Sheet Scaffold")
            } else {
                Text(text = "Close Bottom Sheet Scaffold")
            }
        }
    }
}


@OptIn(ExperimentalMaterialApi::class)
@Preview(showBackground = true)
@Composable
fun MainScreenPreview() {
    val bottomSheetScaffoldState = rememberBottomSheetScaffoldState()
    val scope = rememberCoroutineScope()
    MainScreen(scope = scope, state = bottomSheetScaffoldState)
}
```


> Bottom sheet is an experimental API in Jetpack Compose (by the time I’m writing this tutorial), which means that you have to add the annotation @OptIn(ExperimentalMaterialApi::class) in every function that you’re using it.

Back to the BottomSheetScaffold, let’s add the content for the bottom.

Inside the brackets of the sheetContent we add:


```Kotlin
class MainActivity : ComponentActivity() {

    @OptIn(ExperimentalMaterialApi::class)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val bottomSheetScaffoldState = rememberBottomSheetScaffoldState()
            val scope = rememberCoroutineScope()
            BottomSheetScaffold(
                sheetContent = {
                    BottomSheetContent()
                },
                scaffoldState = bottomSheetScaffoldState,
                sheetShape = RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp),
                sheetBackgroundColor = colorResource(id = R.color.colorPrimary),
                // sheetPeekHeight = 0.dp,
                // scrimColor = Color.Red,  // Color for the fade background when you open/close the bottom sheet
            ) {
                /* Add code later */
            }
        }
    }
}
```

Also, as you see in the code, we made the two top corners rounded and added a background color.

Next, let’s add the MainScreen.

Inside the empty brackets that have left, we create a Scaffold layout, and we add the TopBar, that we made at the beginning, and the MainScreen.




```Kotlin
class MainActivity : ComponentActivity() {

    @OptIn(ExperimentalMaterialApi::class)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val bottomSheetScaffoldState = rememberBottomSheetScaffoldState()
            val scope = rememberCoroutineScope()
            BottomSheetScaffold(
                sheetContent = {
                    BottomSheetContent()
                },
                scaffoldState = bottomSheetScaffoldState,
                sheetShape = RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp),
                sheetBackgroundColor = colorResource(id = R.color.colorPrimary),
                // sheetPeekHeight = 0.dp,
                // scrimColor = Color.Red,  // Color for the fade background when you open/close the bottom sheet
            ) {
                Scaffold(
                    topBar = { TopBar() },
                    backgroundColor = colorResource(id = R.color.colorPrimaryDark)
                ) { padding ->  // We need to pass scaffold's inner padding to content. That's why we use Box.
                     Box(modifier = Modifier.padding(padding)) {
                             MainScreen(scope = scope, state = bottomSheetScaffoldState)
                    }
                }
            }
        }
    }
}
```

#### ModalBottomSheetLayout


In the Compose Activity (e.g., MainActivity.kt), we’re going to add the ModalBottomSheetLayout and create the variables modalBottomSheetState, which saves the state of the modal bottom sheet, and the Coroutine scope that we’ll use in the MainScreen to expand the bottom sheet when we press a button.


```Kotlin
class MainActivity : ComponentActivity() {

    @OptIn(ExperimentalMaterialApi::class)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val modalBottomSheetState = rememberModalBottomSheetState(initialValue = ModalBottomSheetValue.Hidden)
            val scope = rememberCoroutineScope()
            ModalBottomSheetLayout(
                sheetContent = {
                    /* Add code later */
                },
                sheetState = modalBottomSheetState,
            ) {
                /* Add code later */
            }
        }
    }
}

@OptIn(ExperimentalMaterialApi::class)
@Composable
fun MainScreen(scope: CoroutineScope, state: ModalBottomSheetState) {
    Column(
        Modifier
            .fillMaxSize()
            .background(colorResource(R.color.colorPrimaryDark)),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Button(
            colors = ButtonDefaults.buttonColors(
                backgroundColor = colorResource(id = R.color.colorPrimary),
                contentColor = Color.White
            ),
            onClick = {
                scope.launch {
                    state.show()
                }
            }) {
            Text(text = "Open Modal Bottom Sheet Layout")
        }
    }
}

@OptIn(ExperimentalMaterialApi::class)
@Preview(showBackground = true)
@Composable
fun MainScreenPreview() {
    val modalBottomSheetState = rememberModalBottomSheetState(initialValue = ModalBottomSheetValue.Hidden)
    val scope = rememberCoroutineScope()
    MainScreen(scope = scope, state = modalBottomSheetState)
}
```


Back in the ModalBottomSheetLayout, we add the content and make the top corners rounded


```Kotlin
class MainActivity : ComponentActivity() {

    @OptIn(ExperimentalMaterialApi::class)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val modalBottomSheetState = rememberModalBottomSheetState(initialValue = ModalBottomSheetValue.Hidden)
            val scope = rememberCoroutineScope()
            ModalBottomSheetLayout(
                sheetContent = {
                    BottomSheetContent()
                },
                sheetState = modalBottomSheetState,
                sheetShape = RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp),
                sheetBackgroundColor = colorResource(id = R.color.colorPrimary),
                // scrimColor = Color.Red,  // Color for the fade background when you open/close the drawer
            ) {
                /* Add code later */
            }
        }
    }
}
```


Next, we add the TopBar and MainScreen inside a Scaffold layout and we pass the scope and the state for the bottom sheet.




```Kotlin
class MainActivity : ComponentActivity() {

    @OptIn(ExperimentalMaterialApi::class)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val modalBottomSheetState = rememberModalBottomSheetState(initialValue = ModalBottomSheetValue.Hidden)
            val scope = rememberCoroutineScope()
            ModalBottomSheetLayout(
                sheetContent = {
                    BottomSheetContent()
                },
                sheetState = modalBottomSheetState,
                sheetShape = RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp),
                sheetBackgroundColor = colorResource(id = R.color.colorPrimary),
                // scrimColor = Color.Red,  // Color for the fade background when you open/close the drawer
            ) {
                Scaffold(
                    topBar = { TopBar() },
                    backgroundColor = colorResource(id = R.color.colorPrimaryDark)
                ) { padding ->  // We need to pass scaffold's inner padding to content. That's why we use Box.
                     Box(modifier = Modifier.padding(padding)) {
                             MainScreen(scope = scope, state = modalBottomSheetState)
                     }
                }
            }
        }
    }
}

```



