
```Kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            LazyColumnList(this)
        }
    }
}

@Composable
fun LazyColumnList(context: Context){

    LazyColumn {
        items((1..100).toList()){

            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(80.dp)
                    .padding(8.dp)
                    .clickable {
                        Toast
                            .makeText(context, "" + it, Toast.LENGTH_SHORT)
                            .show()
                    },
                elevation = 10.dp
            ) {

                Column(
                    modifier = Modifier.fillMaxSize(),
                    verticalArrangement = Arrangement.Center,
                    horizontalAlignment = Alignment.CenterHorizontally
                ) {
                    Text(text = "Item $it", fontSize = 18.sp)
                }

            }

        }
    }

}
```
