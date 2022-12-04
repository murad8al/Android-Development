
```Kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {

            navigation()

        }
    }
}

@Composable
fun navigation() {

    val navController = rememberNavController()
    NavHost(
        navController = navController,
        startDestination = "splash_screen"
    ) {

        composable("splash_screen") {
            SplashScreen(navController = navController)
        }

        composable("main_screen") {
            Column(
                modifier = Modifier.fillMaxSize(),
                verticalArrangement = Arrangement.Center,
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Text(text = "Main Screen", fontSize = 30.sp, fontWeight = FontWeight.Bold)
            }

        }
    }
}

@Composable
fun SplashScreen(navController: NavController) {

    LaunchedEffect(key1 = true) {
        delay(5000L)
        navController.navigate("main_screen")
    }

    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {

        Image(
            painter = painterResource(id = R.drawable.logo),
            "",
            modifier = Modifier
                .width(200.dp)
                .height(200.dp)
        )
    }
}
```
