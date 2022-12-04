### [medium article]( https://link.medium.com/wBafxzJUqvb)
This article is also available as a video
### [the video](https://youtu.be/0lWyiW-uAuA)

# How To Safely Collect Flows Lifecycle-Aware In Jetpack Compose — A New Approach

in one of my previous articles, I showed you how you can implement your own extension or helper functions to safely collect from Flows while considering the lifecycle of your application.

As time flies by, and because Android libraries get updates on a regular basis, we receive various new features and options that allow us new approaches to how we can deal with specific problems.

With version  <Span style="background-color: #DCDCDC">2.6.0-alpha01</span>   a new extension function got introduced called  <Span style="background-color: #DCDCDC"> collectAsStateWithLifecycle()</span>  . As the name already implies, the function lets us safely collect from a Flow while considering the app’s lifecycle.


Because this makes most of the extension function obsolete I showed in my mentioned article, in this article I want to take up the problem of considering the lifecycle while collecting Flows again and show you how you can use the  <Span style="background-color: #DCDCDC"> collectAsStateWithLifecycle</span>   extension function in action.
 
### 1 What is actually the problem when we ignore the lifecycle?

Speaking of lifecycle awareness, at first, it might not directly be clear what this effectively means for your application.

The term “lifecycle” itself describes the application’s state it's currently in. As you might know, the present Activity can go through six lifecycle states that persist:  <Span style="background-color: #DCDCDC">onCreate() </span>   ,  <Span style="background-color: #DCDCDC"> onStart()</span>   ,  <Span style="background-color: #DCDCDC">onResume() </span>   ,  <Span style="background-color: #DCDCDC"> onPause()</span>   ,  <Span style="background-color: #DCDCDC">onStop() </span>   , and  <Span style="background-color: #DCDCDC"> onDestroy()</span>   .



In order to avoid unexpected behavior and save resources, we only want to collect from our Flow when the app is actually in the foreground and therefore visible to the user.


### 2 An Example of what happens when we ignore the lifecycle

For a better understanding, I thought about a small example that you can practically try out by yourself to experience what it means to consider or not consider the lifecycle.

We start by creating a new  <Span style="background-color: #DCDCDC">ViewModel</span>  .

Consider the following code snippet:

```Kotlin
class TutorialViewModel : ViewModel() {

    private val _exampleCounter = MutableStateFlow(value = 0)
    val exampleCounter = _exampleCounter.asStateFlow()

    init {
        viewModelScope.launch {
            while (true) {
                delay(2000)
                val incrementedCounter: Int = ++_exampleCounter.value
                _exampleCounter.value = incrementedCounter

                Log.e("TutorialViewModel", "Counter invocation: $incrementedCounter")
            }
        }
    }
}
```
We have a  <Span style="background-color: #DCDCDC"> StateFlow<Int></span>   that gets initialized with 0 and then incremented over and over again every 2 seconds.

Additionally, we log every  <Span style="background-color: #DCDCDC">while </span>   loop iteration with the updated counter value to the console.

Next, we add a simple composable that manages the collection of the  <Span style="background-color: #DCDCDC">StateFlow </span>   and finally passes the current counter value to another composable that will display it as a simple  <Span style="background-color: #DCDCDC"> Text</span>  .

```Kotlin
@Composable
private fun TutorialScreen() {

    val tutorialViewModel: TutorialViewModel = viewModel()
    val counter: Int by tutorialViewModel.exampleCounter.collectAsState()
    
    Log.i("TutorialTest","---> counter update: $counter")
    
    TutorialContent(counter)
}

@Composable
private fun TutorialContent(counter: Int) {
    Box(modifier = Modifier.fillMaxSize()) {
        Text(text = "Counter: $counter")
    }
}
```

As you can see we use the ordinary  <Span style="background-color: #DCDCDC"> collectAsState()</span>   function. If we now run the app, we can see the following output in the console:

The “counter update” suddenly disappears when we put the app in the background.

As it seems, the collection stops even if we don’t use the lifecycle-aware collection function. But is that really true?

In fact, the console logs no new outputs because the app doesn’t execute any recompositions in the background. But if we dive in a little bit deeper and go into the actual implementation of the  <Span style="background-color: #DCDCDC">collectAsState() </span>   function, we can set a breakpoint at the collection of a new value.

If you have the app in the foreground, you will see that this breakpoint will hit as soon as an update of our counter  <Span style="background-color: #DCDCDC">StateFlow </span>   gets published.

Because we saw that we receive no console logs while the app is in the background, we might assume that this is also the case for the collection of our flow.


But let’s test it out. Put the app in the background and see what happens.

As you probably see, the breakpoint will hit anyway. And that is exactly the problem. If we app is in the background we don’t want to collect any new values. We only want that to happen when the app is actually in the foreground. Otherwise, this is just a waste of resources.

### 3 Collect Flows Lifecycle Aware

Now that we saw what is actually the problem, let’s talk about how we can avoid it.

Please make sure to add at least version  <Span style="background-color: #DCDCDC"> 2.6.0-alpha01</span>   of the compose lifecycle library to your app-level  <Span style="background-color: #DCDCDC"> build.gradle</span>  dependencies block:


```Kotlin
// Source: https://developer.android.com/jetpack/androidx/releases/lifecycle#2.6.0-alpha01
dependencies {
    def lifecycle_version = "2.6.0-alpha01"

    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    implementation "androidx.lifecycle:lifecycle-viewmodel-compose:$lifecycle_version"
    implementation "androidx.lifecycle:lifecycle-runtime-compose:$lifecycle_version"
}
```


The only thing we need to do now is to replace our  <Span style="background-color: #DCDCDC">collectAsState() </span>   with the lifecycle-aware version  <Span style="background-color: #DCDCDC"> collectAsStateWithLifecycle()</span>  .


```Kotlin

@OptIn(ExperimentalLifecycleComposeApi::class)
@Composable
private fun TutorialScreen() {

    val tutorialViewModel: TutorialViewModel = viewModel()
    val counter: Int by tutorialViewModel.exampleCounter.collectAsStateWithLifecycle()

    Log.i("TutorialTest","---> counter update: $counter")

    TutorialContent(counter)
}
```


If we repeat the experiment from before with the breakpoint now set to the  <Span style="background-color: #DCDCDC"> collectAsStateWithLifecycle()</span>   function, as you can see in the screenshot below, we will see that the breakpoint still gets hit while the app is in the foreground, but no longer when it goes to the background.


### 4 Conclusion

In this article, as a follow-up to one of my previous articles, we have once again addressed the issue of collecting from Flows while considering the app’s lifecycle in an Android app using Jetpack Compose.


Additionally, this time we dived deeper and discussed what ignoring the life cycle means. We saw that the app still collects from our Flow when we use the ordinary  <Span style="background-color: #DCDCDC"> collectAsState() </span>   and that we can fix this issue using the new  <Span style="background-color: #DCDCDC">collectAsStateWithLifecycle() </span>   extension function from the  <Span style="background-color: #DCDCDC"> androidx.lifecycle</span>   library.

That said if you previously used any custom extension or helper functions for collecting from a  <Span style="background-color: #DCDCDC">Flow </span>   or  <Span style="background-color: #DCDCDC">StateFlow </span>   you can now safely replace them with the official provided one.