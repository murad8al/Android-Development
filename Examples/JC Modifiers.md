#### **There are 4 things we can do with modifiers.**
> We use modifiers to decorate or add behavior to Compose UI elements. <br/> We use them for processing user <br/>We can make ui elements interactive using modifiers.
That means using modifiers we can make an ui element clickable, zoomable , draggable or scrollable. <br/>Also, we can use modifiers to add information like accessibility labels to a ui element.

#### Fractions
For these width and height modifiers we can add a fraction value as a parameter. <br/>
Let’s say, we want our text view to have half of the width. <Br/> So, we can add this as o.5f. Here 1f means entire width.
Let’s also set this as o.3f.

```kotlin
 
@Composable
fun Greeting(name: String) {
    Text(
        text = "Hello $name!",
        fontSize = 32.sp,
        fontWeight = FontWeight.Bold,
        color = Color.Red,
        textAlign = TextAlign.Center,
        modifier = Modifier
            .background(color = Color.Yellow)
            .border(2.dp, color = Color.Green)
            .padding(10.dp)
            .fillMaxWidth(0.5f)
            .fillMaxHeight(0.3f)
    )
}
 
```
