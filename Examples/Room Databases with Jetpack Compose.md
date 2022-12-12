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