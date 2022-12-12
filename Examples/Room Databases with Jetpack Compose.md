### Room Databases with Jetpack Compose
This chapter will use the knowledge gained in the chapter entitled [Working with ViewModels in Jetpack Compose](https://www.answertopia.com/jetpack-compose/working-with-viewmodels-in-jetpack-compose/) to provide a detailed tutorial demonstrating how to implement SQLite-based database storage using the Room persistence library. In keeping with the Android architectural guidelines, the project will make use of a view model and repository. The tutorial will also provide a demonstration of all of the elements covered in [Room Databases and Jetpack Compose](https://www.answertopia.com/jetpack-compose/room-databases-and-jetpack-compose/) including entities, a Data Access Object, a Room Database, and asynchronous database queries.

#### About the RoomDemo project
The project created in this chapter is a rudimentary inventory app designed to store the names and quantities of products. When completed, the app will provide the ability to add, delete and search for database entries while also displaying a scrollable list of all products currently stored in the database. This product list will update automatically as database entries are added or deleted. Once completed, the app will appear as illustrated in Figure 43-1 below:


![Figure 43-1](https://github.com/murad8al/GitHub-Images/blob/main/Android-Development/Room%20Databases%20with%20Jetpack%20Compose/Figure%2043-1.jpeg)

#### Creating the RoomDemo project
Launch Android Studio and create a new Empty Compose Activity project named RoomDemo, specifying com.example.roomdemo as the package name, and selecting a minimum API level of API 26: Android 8.0 (Oreo). Within the MainActivity.kt file, delete the Greeting function and add a new empty composable named ScreenSetup which, in turn, calls a function named MainScreen:

```kotlin
@Composable fun ScreenSetup() {    
     MainScreen()
}

@Composable fun MainScreen() {     
}
```

Next, edit the onCreateActivity() method function to call ScreenSetup instead of Greeting. Since this project will be using features that are not supported by the Preview panel, also delete the DefaultPreview composable from the file. To test the project we will be running it on a device or emulator session.

#### Modifying the build configuration
Before adding any new classes to the project, the first step is to add some additional libraries to the build configuration, including the Room persistence library. Locate and edit the module-level build.gradle file (app -> Gradle Scripts -> build.gradle (Module: RoomDemo.app)) and modify it as follows before clicking on the Sync Now link:


```kotlin
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
}
.
.
dependencies {
.
.
    implementation "androidx.room:room-runtime:2.4.2"
    implementation "androidx.room:room-ktx:2.4.2"
    implementation "androidx.compose.runtime:runtime-livedata:1.1.1"
    annotationProcessor "androidx.room:room-compiler:2.4.2"
    kapt "androidx.room:room-compiler:2.4.2"
.
.
}
```

#### Building the entity
This project will begin by creating the entity which defines the schema for the database table. The entity will consist of an integer for the product id, a string column to hold the product name, and another integer value to store the quantity. The product id column will serve as the primary key and will be auto-generated. Table 43-1 summarizes the structure of the entity:

![Table 43-1](https://github.com/murad8al/GitHub-Images/blob/main/Android-Development/Room%20Databases%20with%20Jetpack%20Compose/Table%2043-1.png)

Add a class file for the entity by right-clicking on the app -> java -> com.example.roomdemo entry in the Project tool window and selecting the New -> Kotlin File/Class menu option. In the new class dialog, name the class Product, select the Class entry in the list and press the keyboard return key to generate the file. When the Product.kt file opens in the editor, modify it so that it reads as follows:

```kotlin
package com.example.roomdemo
 
class Product {
 
    var id: Int = 0
    var productName: String = ""
    var quantity: Int = 0
 
    constructor() {}
 
    constructor(id: Int, productname: String, quantity: Int) {
        this.productName = productname
        this.quantity = quantity
    }
    constructor(productname: String, quantity: Int) {
        this.productName = productname
        this.quantity = quantity
    }
}
```

The class now has variables for the database table columns and matching getter and setter methods. Of course, this class does not become an entity until it has been annotated. With the class file still open in the editor, add annotations and corresponding import statements:

```kotlin
package com.example.roomdemo
 
import androidx.annotation.NonNull
import androidx.room.ColumnInfo
import androidx.room.Entity
import androidx.room.PrimaryKey
 
@Entity(tableName = "products")
class Product {
 
    @PrimaryKey(autoGenerate = true)
    @NonNull
    @ColumnInfo(name = "productId")
    var id: Int = 0
 
    @ColumnInfo(name = "productName")
    var productName: String = ""
    var quantity: Int = 0
 
    constructor() {}
 
    constructor(productname: String, quantity: Int) {
        this.id = id
        this.productName = productname
        this.quantity = quantity
    }
}
```

These annotations declare this as the entity for a table named products and assign column names for both the id and name variables. The id column is also configured to be the primary key and auto-generated. Since a primary key can never be null, the @NonNull annotation is also applied. Since it will not be necessary to reference the quantity column in SQL queries, a column name has not been assigned to the quantity variable.

#### Creating the Data Access Object
With the product entity defined, the next step is to create the DAO interface. Referring once again to the Project tool window, right-click on the app -> java -> com.example.roomdemo entry and select the New -> Kotlin File/ Class menu option. In the new class dialog, enter ProductDao into the Name field and select Interface from the list as highlighted in Figure 43-2:

![Figure 43-2](https://github.com/murad8al/GitHub-Images/blob/main/Android-Development/Room%20Databases%20with%20Jetpack%20Compose/Figure%2043-2.jpeg)

Click on OK to generate the new interface and, with the ProductDao.kt file loaded into the code editor, make the following changes:

```kotlin
package com.example.roomdemo
 
import androidx.lifecycle.LiveData
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.Query
 
@Dao
interface ProductDao {
 
    @Insert
    fun insertProduct(product: Product)
 
    @Query("SELECT * FROM products WHERE productName = :name")
    fun findProduct(name: String): List<Product>
 
    @Query("DELETE FROM products WHERE productName = :name")
    fun deleteProduct(name: String)
 
    @Query("SELECT * FROM products")
    fun getAllProducts(): LiveData<List<Product>>
}
```

The DAO implements methods to insert, find and delete records from the products database. The insertion method is passed a Product entity object containing the data to be stored while the methods to find and delete records are passed a string containing the name of the product on which to operate. The getAllProducts() method returns a LiveData object containing all of the records within the database. This method will be used to keep the product list in the user interface layout synchronized with the database.

#### Adding the Room database
tory Tutorial
This chapter will use the knowledge gained in the chapter entitled Working with ViewModels in Jetpack Compose to provide a detailed tutorial demonstrating how to implement SQLite-based database storage using the Room persistence library. In keeping with the Android architectural guidelines, the project will make use of a view model and repository. The tutorial will also provide a demonstration of all of the elements covered in Room Databases and Jetpack Compose including entities, a Data Access Object, a Room Database, and asynchronous database queries.

About the RoomDemo project
The project created in this chapter is a rudimentary inventory app designed to store the names and quantities of products. When completed, the app will provide the ability to add, delete and search for database entries while also displaying a scrollable list of all products currently stored in the database. This product list will update automatically as database entries are added or deleted. Once completed, the app will appear as illustrated in Figure 43-1 below:


Figure 43-1

Creating the RoomDemo project
Launch Android Studio and create a new Empty Compose Activity project named RoomDemo, specifying com.example.roomdemo as the package name, and selecting a minimum API level of API 26: Android 8.0 (Oreo). Within the MainActivity.kt file, delete the Greeting function and add a new empty composable named ScreenSetup which, in turn, calls a function named MainScreen:

@Composable fun ScreenSetup() {    
     MainScreen()
}

@Composable fun MainScreen() {     
}
@Composable fun ScreenSetup() {    
     MainScreen()
}
​
@Composable fun MainScreen() {     
}
Next, edit the onCreateActivity() method function to call ScreenSetup instead of Greeting. Since this project will be using features that are not supported by the Preview panel, also delete the DefaultPreview composable from the file. To test the project we will be running it on a device or emulator session.

 

	You are reading a sample chapter from Jetpack Compose 1.2 Essentials. Buy the full book now in Print or eBook format. Learn more.
Preview  Buy eBook  Buy Print

 

Modifying the build configuration
Before adding any new classes to the project, the first step is to add some additional libraries to the build configuration, including the Room persistence library. Locate and edit the module-level build.gradle file (app -> Gradle Scripts -> build.gradle (Module: RoomDemo.app)) and modify it as follows before clicking on the Sync Now link:

plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
}
.
.
dependencies {
.
.
    implementation "androidx.room:room-runtime:2.4.2"
    implementation "androidx.room:room-ktx:2.4.2"
    implementation "androidx.compose.runtime:runtime-livedata:1.1.1"
    annotationProcessor "androidx.room:room-compiler:2.4.2"
    kapt "androidx.room:room-compiler:2.4.2"
.
.
}
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
}
.
.
dependencies {
.
.
    implementation "androidx.room:room-runtime:2.4.2"
    implementation "androidx.room:room-ktx:2.4.2"
    implementation "androidx.compose.runtime:runtime-livedata:1.1.1"
    annotationProcessor "androidx.room:room-compiler:2.4.2"
    kapt "androidx.room:room-compiler:2.4.2"
.
.
}
Building the entity
This project will begin by creating the entity which defines the schema for the database table. The entity will consist of an integer for the product id, a string column to hold the product name, and another integer value to store the quantity. The product id column will serve as the primary key and will be auto-generated. Table 43-1 summarizes the structure of the entity:

Column

Data Type

productid

Integer / Primary Key / Auto Increment

productname

String

productquantity

Integer

Table 43-1

Add a class file for the entity by right-clicking on the app -> java -> com.example.roomdemo entry in the Project tool window and selecting the New -> Kotlin File/Class menu option. In the new class dialog, name the class Product, select the Class entry in the list and press the keyboard return key to generate the file. When the Product.kt file opens in the editor, modify it so that it reads as follows:

package com.example.roomdemo
 
class Product {
 
    var id: Int = 0
    var productName: String = ""
    var quantity: Int = 0
 
    constructor() {}
 
    constructor(id: Int, productname: String, quantity: Int) {
        this.productName = productname
        this.quantity = quantity
    }
    constructor(productname: String, quantity: Int) {
        this.productName = productname
        this.quantity = quantity
    }
}
The class now has variables for the database table columns and matching getter and setter methods. Of course, this class does not become an entity until it has been annotated. With the class file still open in the editor, add annotations and corresponding import statements:

 

	You are reading a sample chapter from Jetpack Compose 1.2 Essentials. Buy the full book now in Print or eBook format. Learn more.
Preview  Buy eBook  Buy Print

 

package com.example.roomdemo
 
import androidx.annotation.NonNull
import androidx.room.ColumnInfo
import androidx.room.Entity
import androidx.room.PrimaryKey
 
@Entity(tableName = "products")
class Product {
 
    @PrimaryKey(autoGenerate = true)
    @NonNull
    @ColumnInfo(name = "productId")
    var id: Int = 0
 
    @ColumnInfo(name = "productName")
    var productName: String = ""
    var quantity: Int = 0
 
    constructor() {}
 
    constructor(productname: String, quantity: Int) {
        this.id = id
        this.productName = productname
        this.quantity = quantity
    }
}
package com.example.roomdemo
 
import androidx.annotation.NonNull
import androidx.room.ColumnInfo
import androidx.room.Entity
import androidx.room.PrimaryKey
 
@Entity(tableName = "products")
class Product {
 
    @PrimaryKey(autoGenerate = true)
    @NonNull
    @ColumnInfo(name = "productId")
    var id: Int = 0
 
    @ColumnInfo(name = "productName")
    var productName: String = ""
    var quantity: Int = 0
 
    constructor() {}
 
    constructor(productname: String, quantity: Int) {
        this.id = id
        this.productName = productname
        this.quantity = quantity
    }
}
These annotations declare this as the entity for a table named products and assign column names for both the id and name variables. The id column is also configured to be the primary key and auto-generated. Since a primary key can never be null, the @NonNull annotation is also applied. Since it will not be necessary to reference the quantity column in SQL queries, a column name has not been assigned to the quantity variable.

Creating the Data Access Object
With the product entity defined, the next step is to create the DAO interface. Referring once again to the Project tool window, right-click on the app -> java -> com.example.roomdemo entry and select the New -> Kotlin File/ Class menu option. In the new class dialog, enter ProductDao into the Name field and select Interface from the list as highlighted in Figure 43-2:


Figure 43-2

Click on OK to generate the new interface and, with the ProductDao.kt file loaded into the code editor, make the following changes:

package com.example.roomdemo
 
import androidx.lifecycle.LiveData
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.Query
 
@Dao
interface ProductDao {
 
    @Insert
    fun insertProduct(product: Product)
 
    @Query("SELECT * FROM products WHERE productName = :name")
    fun findProduct(name: String): List<Product>
 
    @Query("DELETE FROM products WHERE productName = :name")
    fun deleteProduct(name: String)
 
    @Query("SELECT * FROM products")
    fun getAllProducts(): LiveData<List<Product>>
}
package com.example.roomdemo
 
import androidx.lifecycle.LiveData
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.Query
 
@Dao
interface ProductDao {
 
    @Insert
    fun insertProduct(product: Product)
 
    @Query("SELECT * FROM products WHERE productName = :name")
    fun findProduct(name: String): List<Product>
 
    @Query("DELETE FROM products WHERE productName = :name")
    fun deleteProduct(name: String)
 
    @Query("SELECT * FROM products")
    fun getAllProducts(): LiveData<List<Product>>
}
The DAO implements methods to insert, find and delete records from the products database. The insertion method is passed a Product entity object containing the data to be stored while the methods to find and delete records are passed a string containing the name of the product on which to operate. The getAllProducts() method returns a LiveData object containing all of the records within the database. This method will be used to keep the product list in the user interface layout synchronized with the database.

 

	You are reading a sample chapter from Jetpack Compose 1.2 Essentials. Buy the full book now in Print or eBook format. Learn more.
Preview  Buy eBook  Buy Print

 

Adding the Room database
The last task before adding the repository to the project is to implement the Room Database instance. Add a new class to the project named ProductRoomDatabase, this time with the Class option selected.

Once the file has been generated, modify it as follows using the steps outlined in the [Room Databases and Compose](https://www.answertopia.com/jetpack-compose/room-databases-and-jetpack-compose/) chapter:

```kotlin
package com.example.roomdemo
 
import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase
 
@Database(entities = [(Product::class)], version = 1)
abstract class ProductRoomDatabase: RoomDatabase() {
 
abstract fun productDao(): ProductDao
 
    companion object {
 
        private var INSTANCE: ProductRoomDatabase? = null
 
        fun getInstance(context: Context): ProductRoomDatabase {
            synchronized(this) {
                var instance = INSTANCE
 
                if (instance == null) {
                    instance = Room.databaseBuilder(
                        context.applicationContext,
                        ProductRoomDatabase::class.java,
                        "product_database"
                    ).fallbackToDestructiveMigration()
                        .build()
 
                    INSTANCE = instance
                }
                return instance
            }
        }
    }
}
```

#### Adding the repository
Add a new class named ProductRepository to the project, with the Class option selected.

The repository class will be responsible for interacting with the Room database on behalf of the ViewModel and will need to provide methods that use the DAO to insert, delete and query product records. Except for the getAllProducts() DAO method (which returns a LiveData object) these database operations will need to be performed on separate threads from the main thread.

Remaining within the ProductRepository.kt file, make the following changes :

```kotlin
package com.example.roomdemo
 
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import kotlinx.coroutines.*
 
class ProductRepository(private val productDao: ProductDao) {
 
    val searchResults = MutableLiveData<List<Product>>()
}
```

The above declares a MutableLiveData variable named searchResults into which the results of a search operation are stored whenever an asynchronous search task completes (later in the tutorial, an observer within the ViewModel will monitor this live data object). When an instance of the class is created, it will need to be passed a reference to a ProductDao object.

The repository class now needs to provide some methods that can be called by the ViewModel to initiate database operations. To avoid performing database operations on the main thread, the repository will make use of coroutines where necessary. As such, some additional libraries need to be added to the project before work on the repository class can continue. Start by editing the Gradle Scripts -> build.gradle (Module: RoomDemo.app) file to add the following lines to the dependencies section:

```kotlin
dependencies {
.
.
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.1'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.1'
.
.
}
```

After making the change, click on the Sync Now link at the top of the editor panel to commit the changes.

With a reference to the DAO stored and the appropriate libraries added, the methods are ready to be added to the ProductRepository class file as follows:

```kotlin
.
.
    val searchResults = MutableLiveData<List<Product>>()
    private val coroutineScope = CoroutineScope(Dispatchers.Main)
 
    fun insertProduct(newproduct: Product) {
        coroutineScope.launch(Dispatchers.IO) {
            productDao.insertProduct(newproduct)
        }
    }
 
    fun deleteProduct(name: String) {
        coroutineScope.launch(Dispatchers.IO) {
            productDao.deleteProduct(name)
        }
    }
 
    fun findProduct(name: String) {
        coroutineScope.launch(Dispatchers.Main) {
            searchResults.value = asyncFind(name).await()
        }
    }
 
    private fun asyncFind(name: String): Deferred<List<Product>?> =
        coroutineScope.async(Dispatchers.IO) {
            return@async productDao.findProduct(name)
        }
.
.
```

In the case of the find operation, the asyncFind() method makes use of a deferred value to return the search results to the findProduct() method. Because the findProduct() method needs access to the searchResults variable, the call to the asyncFind() method is dispatched to the main thread which, in turn, performs the database operation using the IO dispatcher.

One final task remains to complete the repository class. The LazyColumn which will be added to the user interface layout later will need to be able to keep up to date with the current list of products stored in the database. The ProductDao class already includes a method named getAllProducts() which uses a SQL query to select all of the database records and return them wrapped in a LiveData object. The repository needs to call this method once on initialization and store the result within a LiveData object that can be observed by the ViewModel and, in turn, by the main activity. Once this has been set up, each time a change occurs to the database table the activity observer will be notified and the LazyColumn recomposed with the latest product list. Remaining within the ProductRepository.kt file, add a LiveData variable and call to the DAO getAllProducts() method:


```kotlin
.
.
class ProductRepository(private val productDao: ProductDao) {
 
    val allProducts: LiveData<List<Product>> = productDao.getAllProducts()
    val searchResults = MutableLiveData<List<Product>>()
.
.
```

#### Adding the ViewModel
The ViewModel will be responsible for the creation of the database, DOA, and repository instances and for providing methods and LiveData objects that can be utilized by the UI controller to handle events.

Start by editing the build.gradle (Module RoomDemo.app) file to add the view model lifecycle library:

```kotlin
.
.
dependencies {
.
.
    implementation 'androidx.lifecycle:lifecycle-viewmodel-compose:2.4.1'
.
.
```

Sync the project before adding a ViewModel class to the project by right-clicking on the app -> java -> com. example.roomdemo entry in the Project tool window and selecting the New -> Kotlin File/Class menu option. In the New Class dialog, name the class MainViewModel, select the Class entry in the list and press the keyboard return key to generate the file.

Within the MainViewModel.kt file, modify the class declaration to accept an application context instance together with some properties and an initializer block as outlined below. The application context, represented by the Android Context class, is used in application code to gain access to the application resources at runtime. In addition, a wide range of methods may be called on an application’s context to gather information and make changes to the application’s environment. In this case, the application context is required when creating a database and will be passed into the view model from within the activity later in the chapter:

```kotlin
.
.
import android.app.Application
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
.
.
class MainViewModel(application: Application) : ViewModel() {
 
    val allProducts: LiveData<List<Product>>
    private val repository: ProductRepository
    val searchResults: MutableLiveData<List<Product>>
 
    init {
        val productDb = ProductRoomDatabase.getInstance(application)
        val productDao = productDb.productDao()
        repository = ProductRepository(productDao)
 
        allProducts = repository.allProducts
        searchResults = repository.searchResults
    }
}
```

The initializer block creates a database which is used to create a DAO instance. We then use the DAO to initialize the repository:

```kotlin
val productDb = ProductRoomDatabase.getInstance(application)
val productDao = productDb.productDao()
repository = ProductRepository(productDao)
```

Finally, the repository is used to store references to the search results and allProducts live data objects so that they can be converted to states later within the main activity:

```kotlin
allProducts = repository.allProducts
searchResults = repository.searchResults
```

All that now remains within the ViewModel is to implement the methods that will be called from within the activity in response to button clicks. These need to be placed after the init block as follows:

```kotlin
.
.
init {
.
.
}
 
fun insertProduct(product: Product) {
    repository.insertProduct(product)
}
 
fun findProduct(name: String) {
    repository.findProduct(name)
}
 
fun deleteProduct(name: String) {
    repository.deleteProduct(name)
}
.
.
```

#### Designing the user interface
With the database, DOA, repository, and ViewModel completed, we are now ready to design the user interface. Start by editing the MainActivity.kt file and adding three composables to be used as the input text fields, column rows, and column title:

```kotlin
.
.
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.*
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
.
.
class MainActivity : ComponentActivity() {
.
.
@Composable
fun TitleRow(head1: String, head2: String, head3: String) {
    Row(
        modifier = Modifier
            .background(MaterialTheme.colors.primary)
            .fillMaxWidth()
            .padding(5.dp)
    ) {
        Text(head1, color = Color.White,
            modifier = Modifier
            .weight(0.1f))
        Text(head2, color = Color.White,
            modifier = Modifier
                .weight(0.2f))
        Text(head3, color = Color.White,
            modifier = Modifier.weight(0.2f))
    }
}
 
@Composable
fun ProductRow(id: Int, name: String, quantity: Int) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(5.dp)
    ) {
        Text(id.toString(), modifier = Modifier
            .weight(0.1f))
        Text(name, modifier = Modifier.weight(0.2f))
        Text(quantity.toString(), modifier = Modifier.weight(0.2f))
    }
}
 
@Composable
fun CustomTextField(
    title: String,
    textState: String,
    onTextChange: (String) -> Unit,
    keyboardType: KeyboardType
) {
    OutlinedTextField(
        value = textState,
        onValueChange = { onTextChange(it) },
        keyboardOptions = KeyboardOptions(
            keyboardType = keyboardType
        ),
        singleLine = true,
        label = { Text(title)},
        modifier = Modifier.padding(10.dp),
        textStyle = TextStyle(fontWeight = FontWeight.Bold,
            fontSize = 30.sp)
    )
}
```

#### Writing a ViewModelProvider Factory class
The view model we have created in this chapter is slightly more complex than earlier examples because it expects to be passed a reference to the Application instance. Previously we have used the viewModel() function to create view models. Unfortunately, the viewModel() function will not allow us to simply pass through the Application reference as an argument when we call it. Instead, we need to pass the function a custom ViewModelProvider Factory class designed to accept an Application reference and return an initialized MainViewModel instance.

Within the MainActivity.kt file, add the following factory class at the end of the file after the last closing brace (}):

```kotlin
.
.
import android.app.Application
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider
.
.
class MainViewModelFactory(val application: Application) : 
                                    ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return MainViewModel(application) as T
    }
}
```

In addition to the factory, the viewModel() function also requires a reference to the current ViewModelStoreOwner. The view model store can be thought of as a container in which all currently active view models are stored together with an identifying string for each model (which also needs to be passed to the viewModel() call). Remaining the MainActivity.kt file, locate the onCreate() method, and modify it so that it reads as follows:

```kotlin
.
.
import androidx.compose.ui.platform.LocalContext
import androidx.lifecycle.viewmodel.compose.LocalViewModelStoreOwner
import androidx.lifecycle.viewmodel.compose.viewModel
.
.
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent {
        RoomDemoTheme {
            // A surface container using the 'background' color from the theme
            Surface(
                modifier = Modifier.fillMaxSize(),
                color = MaterialTheme.colors.background
            ) {
 
                val owner = LocalViewModelStoreOwner.current

                owner?.let {
                    val viewModel: MainViewModel = viewModel(
                        it,
                        "MainViewModel",
                        MainViewModelFactory(
                          LocalContext.current.applicationContext 
                                                      as Application)
                    )
 
                    ScreenSetup(viewModel)
                }
            }
        }
    }
}
```

The added code begins by obtaining a reference to the current local view model store owner. After checking the owner is not null, the viewModel() function is called and passed the owner, an identifying string, and view model factory (to which is passed the Application reference). The view model returned by the viewModel() call is then passed to ScreenSetup.

Next, modify ScreenSetup to accept the ViewModel and use it to convert the allProducts and searchResults live data objects to state values initialized with empty lists. These states, together with the view model also need to be passed to the MainScreen composable:

```kotlin
.
.
import androidx.compose.runtime.*
import androidx.compose.runtime.livedata.observeAsState
.
.

@Composable
fun ScreenSetup(viewModel: MainViewModel) {
 
    val allProducts by viewModel.allProducts.observeAsState(listOf())
    val searchResults by viewModel.searchResults.observeAsState(listOf())
 
    MainScreen(
        allProducts = allProducts,
        searchResults = searchResults,
        viewModel = viewModel
    )
}

@Composable
fun MainScreen(
    allProducts: List<Product>,
    searchResults: List<Product>,
    viewModel: MainViewModel
) {
 
}
```

When creating the ViewModel instance above, note that we used the LocalContext object to obtain a reference to the application context and passed it to the view model so that it can be used when creating the database.

#### Completing the MainScreen function
Within the MainScreen function, add some state and event handler declarations as follows:

```kotlin
var productName by remember { mutableStateOf("") }
    var productQuantity by remember { mutableStateOf("") }
    var searching by remember { mutableStateOf(false) }
 
    val onProductTextChange = { text : String ->
        productName = text
    }
 
    val onQuantityTextChange = { text : String ->
        productQuantity = text
    }
}
```

Continue modifying the MainScreen function to add a Column containing two CustomTextField composables and a Row containing four Button components as follows:

```kotlin
.
import androidx.compose.ui.Alignment.Companion.CenterHorizontally
.
.
@Composable
fun MainScreen(
    allProducts: List<Product>, 
    searchResults: List<Product>, 
    viewModel: MainViewModel
) {
.
.
    Column(
        horizontalAlignment = CenterHorizontally,
        modifier = Modifier
            .fillMaxWidth()
    ) {
        CustomTextField(
            title = "Product Name",
            textState = productName,
            onTextChange = onProductTextChange,
            keyboardType = KeyboardType.Text
        )
 
        CustomTextField(
            title = "Quantity",
            textState = productQuantity,
            onTextChange = onQuantityTextChange,
            keyboardType = KeyboardType.Number
        )
 
        Row(
            horizontalArrangement = Arrangement.SpaceEvenly,
            modifier = Modifier
                .fillMaxWidth()
                .padding(10.dp)
        ) {
            Button(onClick = {
                if (productQuantity.isNotEmpty()) {
                    viewModel.insertProduct(
                        Product(
                            productName,
                            productQuantity.toInt()
                        )
                    )
                    searching = false
                }
            }) {
                Text("Add")
            }
 
            Button(onClick = {
                searching = true
                viewModel.findProduct(productName)
            }) {
                Text("Search")
            }
 
            Button(onClick = {
                searching = false
                viewModel.deleteProduct(productName)
            }) {
                Text("Delete")
            }
 
            Button(onClick = {
                searching = false
                productName = ""
                productQuantity = ""
            }) {
                Text("Clear")
            }
        }
    }
}
```

Finally, add a LazyColumn to the parent Column immediately after the row of Button components. This will display a single instance of the TitleRow followed by a ProductRow for each product. The searching state will be used to decide whether the list is to include all products or only those products that match the search criteria:

```kotlin
.
.
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
.
.
@Composable
fun MainScreen(allProducts: List<Product>, searchResults: List<Product>, viewModel: MainViewModel) {
.
.
        LazyColumn(
            Modifier
                .fillMaxWidth()
                .padding(10.dp)
        ) {
            val list = if (searching) searchResults else allProducts
 
            item {
                TitleRow(head1 = "ID", head2 = "Product", head3 = "Quantity")
            }
 
            items(list) { product ->
                ProductRow(id = product.id, name = product.productName, 
                                 quantity = product.quantity)
            }
        }
    }
}
```

#### Testing the RoomDemo app
Compile and run the app on a device or emulator where it should appear as illustrated in Figure 43-1 above. Add some products and make sure that they appear automatically in the LazyColumn. Perform a search for an existing product and verify that the matching result is listed. Click the Clear button to reset the list, then enter the name for an existing product, delete it from the database and confirm that it is removed from the product list.

#### Using the Database Inspector
As previously outlined in “Room Databases and Compose”, the Database Inspector tool may be used to inspect the content of Room databases associated with a running app and to perform minor data changes. After adding some database records using the RoomDemo app, display the Database Inspector tool using the View -> Tool Windows -> App Inspection menu option:
From within the inspector window, select the running app from the menu marked A in Figure 43-3 below:

![Figure 43-3](https://github.com/murad8al/GitHub-Images/blob/main/Android-Development/Room%20Databases%20with%20Jetpack%20Compose/Figure%2043-3.jpeg)

From the Databases panel (B) double-click on the products table to view the table rows currently stored in the database. Enable the Live updates option (C) and then use the running app to add more records to the database. Note that the Database Inspector updates the table data (D) in real-time to reflect the changes.

Turn off Live updates so that the table is no longer read-only, double-click on the quantity cell for a table row, and change the value before pressing the keyboard Enter key. Return to the running app and search for the product to confirm the change made to the quantity in the inspector was saved to the database table.

Finally, click on the table query button (indicated by the arrow in Figure 43-4 below) to display a new query tab (A), make sure that product_database is selected (B), and enter a SQL statement into the query text field (C) and click the Run button(D):

![Figure 43-4](https://github.com/murad8al/GitHub-Images/blob/main/Android-Development/Room%20Databases%20with%20Jetpack%20Compose/Figure%2043-4.jpeg)

The list of rows should update to reflect the results of the SQL query (E).

#### Summary
This chapter has demonstrated the use of the Room persistence library to store data in an SQLite database. The finished project made use of a repository to separate the ViewModel from all database operations and demonstrated the creation of entities, a DAO, and a room database instance, including the use of asynchronous tasks when performing some database operations.
