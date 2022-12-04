# Standard Button
### This Button Jetpack Compose component has two compulsory attributes.  <Span style="background-color: #C0C0C0">OnClick</span>   and  <Span style="background-color: #C0C0C0">text </span>  .

### OnClick will be called when the user clicked on the button. And, as usual we use Text to display a text on the button.

### Let’s go ahead and create a Button.


```Kotlin
Button(onClick = {})
    {
        Text("Standard")
    }
```

### Next, I am going add a Toast message for click event. But, in order to do that, we need to get the context. Inside Jetpack Compose components  we would use <span style="background-color: #C0C0C0">LocalContext.current</span> to get the context.


```Kotlin
@Composable
fun ButtonDemo() {
    val context = LocalContext.current
    Button(onClick = {
        Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
    })
    {
        Text("Standard")
    }
}
```

## Disabling a Button

#### By default any button is in the enabled state. A jetpack compose button can be disabled by adding <Span style="background-color: #C0C0C0">enabled = false</span> attribute.
#### For the comparison, here I created a disabled button. It would not show the toast message.


```Kotlin
Button(
        onClick = {
            Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
        },
        enabled = false
    ) {
        Text("Add To Cart")
    }
```

## Text Button
#### A text button is a button without a background shape or a border. We can recognize it as, just a text component within a Button component.


```Kotlin
TextButton(onClick = {
        Toast.makeText(context, "Clicked on Text Button", Toast.LENGTH_SHORT).show()
    }) {
        Text("  Text  ")
    }
```

## `Outlined Button`


#### Only difference between a normal button and an out lined button is the stroke being used to outline the body of the button.


```Kotlin
OutlinedButton(onClick = {
        Toast.makeText(context, "Clicked on Out Lined Button", Toast.LENGTH_SHORT).show()
    }) {
        Text("Outlined")
    }
```

## Icon Button

#### We have another button type named IconButton. It is a just a clickable icon. We would use an icon button to represent actions  such as refresh, download, upload and delete. Let’s go ahead and create an Icon Button.


```Kotlin
IconButton(onClick = {
        Toast.makeText(context, "Clicked on Icon Button", Toast.LENGTH_SHORT).show()
    }) {
        Icon(
            Icons.Filled.Refresh,
            contentDescription = "Refresh Button"
        )
    }
```
#### Instead of Text, this has Icon component.  Icon has two compulsory attributes. The image and the content description.
#### For the image we can provide our own image using painter. Or we can just use a vector graphic provided by the android studio.
#### Here, I have used the refresh vector image. 

#### An Icon button usually takes a space of a square of minimum size is 48x48dp. However, we can change its attributes.
#### So, let’s go ahead and add a color. Then, let’s also use size modifier to increase the size.

```Kotlin
IconButton(onClick = {
        Toast.makeText(context, "Clicked on Icon Button", Toast.LENGTH_SHORT).show()
    }) {
        Icon(
            Icons.Filled.Refresh,
            contentDescription = "Refresh Button",
            tint = Color.DarkGray,
            modifier = Modifier.size(80.dp)
        )
    }
```
#### Here is the complete  <Span style="background-color: #C0C0C0">MainActivity.kt</span>   file at this point of the tutorial.


```Kotline
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Column(
                verticalArrangement = Arrangement.SpaceEvenly,
                horizontalAlignment = Alignment.CenterHorizontally,
                modifier = Modifier.fillMaxSize()
            ) {
                ButtonDemo()
            }
        }
    }
} 
@Composable
fun ButtonDemo() {
 
    val context = LocalContext.current
 
    Button(onClick = {
        Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
    })
    {
        Text("Standard")
    }
 
    Button(
        onClick = {
            Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
        },
        enabled = false
    ) {
        Text("Disabled")
    }
 
    TextButton(onClick = {
        Toast.makeText(context, "Clicked on Text Button", Toast.LENGTH_SHORT).show()
    }) {
        Text("  Text  ")
    }
 
    OutlinedButton(onClick = {
        Toast.makeText(context, "Clicked on Out Lined Button", Toast.LENGTH_SHORT).show()
    }) {
        Text("Outlined")
    }
 
    IconButton(onClick = {
        Toast.makeText(context, "Clicked on Icon Button", Toast.LENGTH_SHORT).show()
    }) {
        Icon(
            Icons.Filled.Refresh,
            contentDescription = "Refresh Button",
            tint = Color.DarkGray,
            modifier = Modifier.size(80.dp)
        )
    }
} 
```

## Customizing Buttons.

#### Sometimes we would need to customize the buttons to compatible with the app’s theme. 

#### In this example I have added content padding. Then a border of black color. And, I have also added background and content color using “colors” attribute.

```Kotlin
Button(onClick = {
        Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
    },
        contentPadding = PaddingValues(16.dp),
        border = BorderStroke(10.dp, Color.Black),
        colors = ButtonDefaults.textButtonColors(
            backgroundColor = Color.DarkGray,
            contentColor = Color.White
        )
    ) {
        Text("Add To Cart",
            style = MaterialTheme.typography.h3,
            modifier = Modifier.padding(5.dp)
        )
    }
```
## Changing the Shape of the Button.

#### We would easily change the shape of a jetpack compose buttons using “shape” attribute.

### 1. Cut Corner Shape.

```Kotlin
Button(onClick = {
        Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
    },
        shape = CutCornerShape(10.dp),
        contentPadding = PaddingValues(16.dp),
        border = BorderStroke(10.dp, Color.Black),
        colors = ButtonDefaults.textButtonColors(
            backgroundColor = Color.DarkGray,
            contentColor = Color.White
        )
    ) {
        Text("Add To Cart",
            style = MaterialTheme.typography.h3,
            modifier = Modifier.padding(5.dp)
        )
    }
```
### 2. Circle Shape

```Kotlin
Button(onClick = {
        Toast.makeText(context, "Clicked on Button", Toast.LENGTH_SHORT).show()
    },
        shape = CircleShape,
        contentPadding = PaddingValues(16.dp),
        border = BorderStroke(10.dp, Color.Black),
        colors = ButtonDefaults.textButtonColors(
            backgroundColor = Color.DarkGray,
            contentColor = Color.White
        )
    ) {
        Text("Add To Cart",
            style = MaterialTheme.typography.h3,
            modifier = Modifier.padding(5.dp)
        )
    }
```



