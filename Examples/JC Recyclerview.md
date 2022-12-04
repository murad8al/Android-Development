#### Displaying a vertical List using Column

Here is a simple code sample, in which I am trying to display a list of 100 items in a Colum.


```Kotlin
 
@Composable
fun ColumnDemo() {
 
    Column {
        for (i in 1..100) {
            Text(
                "User Name $i",
                style = MaterialTheme.typography.h3,
                modifier = Modifier.padding(10.dp)
            )
 
            Divider(color = Color.Black, thickness = 5.dp)
 
        }
    }
}
```

Even thought this creates 100 items in the memory, we will not be able to see all of them. Because column is not scrollable.


#### Scrollable Column

To make a column scrollable, We need to do two things.

we need to get an instance of ScrollState.
Which will be used to remember the scrolled position of the list for navigation back situations.
we need to add the verticalScroll modifier to the column passing the scroll state instance.
Use rememberScrollState to get an instance of ScrollState. val scrollState = rememberScrollState() 

Then, add the modifier. modifier = Modifier.verticalScroll(scrollState).


```Kotlin
 
@Composable
fun ScrollableColumnDemo() {
    val scrollState = rememberScrollState()
    Column(
        modifier = Modifier.verticalScroll(scrollState)
    ) {
        for (i in 1..100) {
            Text(
                "User Name $i",
                style = MaterialTheme.typography.h3,
                modifier = Modifier.padding(10.dp)
            )
            Divider(color = Color.Black, thickness = 5.dp)
        }
    }
}
 
```
####  The problem with Scrollable column.


This scrollable column is the jetpack compose equivalent  to ListView.

The problem with this approach is that it composes and emits all the list items to the screen, to the scrollable column, whether we look at them all or not.

That is not an efficient or effective way of doing it.

Think, what would happen if there are 10000 list items with images? That would negatively affect to the performance of the app.

####  Lazy Column

The solution to above problem with scrollable column is LazyColumn.

LazyColumn, only composes and emits ,  items that are required to view current screen.

It’s the Recyclerview equivalent in jetpack compose.

So, let’s go ahead and create a LazyColumn.


```Kotlin
 
@Composable
fun LazyColumnDemo() {
    LazyColumn {
        items(100) {
            Text(
                "User Name $it",
                style = MaterialTheme.typography.h3,
                modifier = Modifier.padding(10.dp)
            )
            Divider(color = Color.Black, thickness = 5.dp)
        }
    }
}
 
```


Lazy column has a function named items.

Instead of using a for loop, now we are using items function to get the count for 100 items.

If you invoke this function from withing setContent, you would see a simialar scrollable list.

But this time you would experience a slightly improved performance.

####  Implementing the Item Click Listener for Lazy Column in Jetpack Compose.


Now, I am going to show you how to implement a list item click listener for a lazy column.

We can do it very easily using kotlin lambda expression and higher order functions.

To implement a click listener, we will do two things.

 Add a lambda expression as a parameter to the composable. 
Pass the selected list item’s value to the anonymous function using clickable modifier.

We will add a lambda expression as a parameter to the composable.

selectedItem: (String) -> (Unit) 

Input data type of it must be the data type of selected item.

In our case we will select an string. So the input data type should be string.

If we are selecting an User object, then the input data type of the lambda expression would be User.

After that, we would pass the selected list item’s value to the anonymous function defined by the lambda expression using clickable modifier.

.clickable { selectedItem("$it Selected") } 



```Kotlin
@Composable
fun LazyColumnDemo2(selectedItem: (String) -> (Unit)) {
    LazyColumn {
        items(100) {
            Text(
                "User Name $it",
                style = MaterialTheme.typography.h3,
                modifier = Modifier
                    .padding(10.dp)
                    .clickable { selectedItem("$it Selected") }
            )
            Divider(color = Color.Black, thickness = 5.dp)
        }
    }
}
```

####  Jetpack Compose RecyclerView(LazyColumn) with Material Design

So, now you have learnt all the fundamentals you need.

Let’s go ahead and design a more advanced, good looking recycler view.

For that I am using a set of images of popular tvSows. 

#### Images we use for the list

open the final project with your android studio.

Res =>Drawable=>Copy these images.

Then go to current project.

Res=>Drawable=>Save them all

####  Data class

We will also need a data class to represent a single TvShow entity.

Create a new Kotlin class and name it as “TvShow”

#### TvShow.kt

```Kotlin
import java.io.Serializable
 
data class TvShow(
    val id: Int,
    val name: String,
    val year: Int,
    val rating : Double,
    val imageId : Int,
    val overview : String
    ) : Serializable
```

And, also notice,

This data class implements the Serializable Interface. Which Is required when we are using Intent to transform a TvShow object from one activity to another.

Next, create a new Kotlin object and name it as TvShowList. Copy and past this code to it.

 

TvShowList.kt


```Kotlin
object TvShowList {
 
    val tvShows = listOf(
 
        TvShow(
            id = 1,
            name = "Lucifer",
            year = 2016,
            rating = 8.1,
            imageId = R.drawable.lucifer,
            overview = "Bored and unhappy as the Lord of Hell, Lucifer Morningstar abandoned his throne and retired to Los Angeles, where he has teamed up with LAPD detective Chloe Decker to take down criminals. But the longer he's away from the underworld, the greater the threat that the worst of humanity could escape."
        ),
        TvShow(
            id = 2,
            name = "Ragnarok",
            year = 2020,
            rating = 7.5,
            imageId = R.drawable.ragnarok,
            overview = "A small Norwegian town experiencing warm winters and violent downpours seems to be headed for another Ragnarök -- unless someone intervenes in time."
        ),
        TvShow(
            id = 3,
            name = "The Flash",
            year = 2014,
            rating = 7.7,
            imageId = R.drawable.flash,
            overview = "After a particle accelerator causes a freak storm, CSI Investigator Barry Allen is struck by lightning and falls into a coma. Months later he awakens with the power of super speed, granting him the ability to move through Central City like an unseen guardian angel. Though initially excited by his newfound powers, Barry is shocked to discover he is not the only \"meta-human\" who was created in the wake of the accelerator explosion -- and not everyone is using their new powers for good. Barry partners with S.T.A.R. Labs and dedicates his life to protect the innocent. For now, only a few close friends and associates know that Barry is literally the fastest man alive, but it won't be long before the world learns what Barry Allen has become...The Flash."
        ),
        TvShow(
            id = 4,
            name = "The Good Doctor",
            year = 2017,
            rating = 7.1,
            imageId = R.drawable.doctor,
            overview = "A young surgeon with Savant syndrome is recruited into the surgical unit of a prestigious hospital. The question will arise: can a person who doesn't have the ability to relate to people actually save their lives"
        ),
        TvShow(
            id = 5,
            name = "Grey's Anatomy",
            year = 2005,
            rating = 7.5,
            imageId = R.drawable.anatomy,
            overview = "Follows the personal and professional lives of a group of doctors at Seattle’s Grey Sloan Memorial Hospital."
        ),
        TvShow(
            id = 6,
            name = "The Walking Dead",
            year = 2010,
            rating = 8.2,
            imageId = R.drawable.twd,
            overview = "Sheriff's deputy Rick Grimes awakens from a coma to find a post-apocalyptic world dominated by flesh-eating zombies. He sets out to find his family and encounters many other survivors along the way."
        ),
        TvShow(
            id = 7,
            name = "Carnival Row",
            year = 2019,
            rating = 7.9,
            imageId = R.drawable.carnival,
            overview = "In a mystical and dark city filled with humans, fairies and other creatures, a police detective investigates a series of gruesome murders leveled against the fairy population. During his investigation, the detective becomes the prime suspect and must find the real killer to clear his name."
        ),
        TvShow(
            id = 8,
            name = "Legacies",
            year = 2018,
            rating = 7.4,
            imageId = R.drawable.legacies,
            overview = "In a place where young witches, vampires, and werewolves are nurtured to be their best selves in spite of their worst impulses, Klaus Mikaelson’s daughter, 17-year-old Hope Mikaelson, Alaric Saltzman’s twins, Lizzie and Josie Saltzman, among others, come of age into heroes and villains at The Salvatore School for the Young and Gifted."
        )
 
    )
}
 
```

Now, let’s start creating our composables.

We could write all the codes to display our data in a single disposable.

That’s what we have being doing during this tutorial for our previous LazyColumn and Scrollable Column examples.

But if we do so, since this layout has a lot of components it would be a very difficult to read, difficult to maintain code.

Therefore following the best practices I am going write this code as 3 separate composables.

So, Let’s start by creating a composable for showing the TvShow image. This should take an instance of TvShow as an argument.

Create a new Kotlin file . And, name it as TvShowImage.kt .

 

```Kotlin
 
@Composable
fun TvShowImage(tvShow: TvShow) {
    Image(
        painter = painterResource(id = tvShow.imageId),
        contentDescription = null,
        contentScale = ContentScale.Crop,
        modifier = Modifier
            .padding(4.dp)
            .height(140.dp)
            .width(100.dp)
            .clip(RoundedCornerShape(corner = CornerSize(10.dp)))
 
    )
}
 
```

Now create another kotlin file named TvShowListItem.


#### TvShowListItem.kt

```Kotlin

@Composable
fun TvShowListItem(tvShow: TvShow, selectedItem: (TvShow) -> Unit) {
    Card(
        modifier = Modifier
            .padding(10.dp)
            .fillMaxWidth(),
        elevation = 10.dp,
        shape = RoundedCornerShape(corner = CornerSize(10.dp))
    ) {
        Row(
            modifier = Modifier
                .padding(5.dp)
                .fillMaxWidth()
                .clickable { selectedItem(tvShow) },
            verticalAlignment = Alignment.CenterVertically
        ) {
            TvShowImage(tvShow = tvShow)
            Column {
                Text(text = tvShow.name, style = MaterialTheme.typography.h5)
                Spacer(modifier = Modifier.height(4.dp))
                Text(
                    text = tvShow.overview,
                    style = MaterialTheme.typography.body1,
                    maxLines = 3,
                    overflow = TextOverflow.Ellipsis
                )
                Spacer(modifier = Modifier.height(8.dp))
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Text(text = tvShow.year.toString(), style = MaterialTheme.typography.h5)
                    Text(text = tvShow.rating.toString(), style = MaterialTheme.typography.h5)
                }
 
            }
 
        }
 
    }
}
 
```

Now, go back to MainActivity.kt .And, add this composable there to display the list of TvShows. 



```Kotlin
 
@Composable
fun DisplayTvShows(selectedItem: (TvShow) -> Unit) {
 
    val tvShows = remember { TvShowList.tvShows }
    LazyColumn(
        contentPadding = PaddingValues(horizontal = 16.dp, vertical = 8.dp)
    ) {
        items(
            items = tvShows,
            itemContent = {
                TvShowListItem(tvShow = it, selectedItem)
            }
        )
    }
 
}
 
```

In jetpack compose we have a composable named remember.

Composable functions can store a single object in memory by using the remember composable.

A value computed by remember is stored during the initial composition, and returned during recomposition.

So, following best practices , I have used remember composable in above code.

Next, we will invoke DisplayTvShows()  from the setContent bock of onCreate(). 

For now, let’s just add a Toast message to display the name of the selected TvShow.



```Kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            DisplayTvShows {
                Toast.makeText(this, it.name,Toast.LENGTH_LONG).show()
            }
        }
    }
}
```


Note : If your status bar showing a different color, you could change it by changing colors in themes.xml file.(res=>values=>themes>themes.xml)



####  Navigate to second activity with selected data object

Let’s create a new activity. Select “Empty Compose Activity” . And, name it as InfoActivity.

Next, we will create a  composable function to display full information of the selected tv show. 

This function would take an instance of TvShow as an argument.


```Kotlin
@Composable
fun ViewMoreInfo(tvShow: TvShow) {
    val scrollState = rememberScrollState()
    Card(
        modifier = Modifier.padding(10.dp),
        elevation = 10.dp,
        shape = RoundedCornerShape(corner = CornerSize(10.dp))
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .verticalScroll(scrollState)
                .padding(10.dp)
        ) {
            Image(
                painter = painterResource(id = tvShow.imageId),
                contentDescription = null,
                modifier = Modifier
                    .fillMaxWidth()
                    .clip(shape = RoundedCornerShape(4.dp)),
                contentScale = ContentScale.Fit
            )
            Spacer(modifier = Modifier.height(16.dp))
            Text(
                text = tvShow.name,
                style = MaterialTheme.typography.h3
            )
            Spacer(modifier = Modifier.height(16.dp))
            Text(
                text = tvShow.overview,
                style = MaterialTheme.typography.h5
            )
            Spacer(modifier = Modifier.height(16.dp))
            Text(
                text = "Original release : ${tvShow.year}",
                style = MaterialTheme.typography.h5
            )
            Spacer(modifier = Modifier.height(16.dp))
            Text(
                text = "IMDB : ${tvShow.rating}",
                style = MaterialTheme.typography.h5
            )
 
        }
    }
```

####  Using “Intent” to transform an object between activities


Next, we need to create a companion object. This will be used to transform the selected item to the InfoActivity.


```Kotlin
companion object{
        private const val TvShowId = "tvshowid"
        fun intent(context: Context,tvShow: TvShow)=
            Intent(context,InfoActivity::class.java).apply {
                putExtra(TvShowId,tvShow)
            }
    }
```

Then, we will define a reference variable of type TvShow to get that transformed TvShow object.



```Kotlin
private val tvShow : TvShow by lazy {
        intent?.getSerializableExtra(TvShowId) as TvShow
    }
```

Inside the onCreate() function’s setContent block we need to write codes to invoke ViewMoreInfo() passing that TvShow instance.



```Kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
           ViewMoreInfo(tvShow = tvShow)
        }
    }
```


After that, let’s go back to MainActivity and writec codes to send the selected list item(TvSow object) to the InfoActivity.



```Kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {     
            DisplayTvShows {            
                startActivity(InfoActivity.intent(this,it))
            }
        }
    }
}
```




