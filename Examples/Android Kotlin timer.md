####  How to create a simple countdown timer in Kotlin?

---


You can use Kotlin objects:

 ```Kotlin
val timer = object: CountDownTimer(20000, 1000) {
     override fun onTick(millisUntilFinished: Long) {...}

    override fun onFinish() {...}
 }
 timer.start()
```
---

I've solved my problem with timer in Kotlin like this:

```Kotlin
class Timer {

    private val job = SupervisorJob()
    private val scope = CoroutineScope(Dispatchers.Default + job)

    private fun startCoroutineTimer(delayMillis: Long = 0, repeatMillis: Long = 0, action: () -> Unit) = scope.launch(Dispatchers.IO) {
        delay(delayMillis)
        if (repeatMillis > 0) {
            while (true) {
                action()
                delay(repeatMillis)
            }
        } else {
            action()
        }
    }

    private val timer: Job = startCoroutineTimer(delayMillis = 0, repeatMillis = 20000) {
        Log.d(TAG, "Background - tick")
        doSomethingBackground()
        scope.launch(Dispatchers.Main) {
            Log.d(TAG, "Main thread - tick")
            doSomethingMainThread()
        }
    }

    fun startTimer() {
        timer.start()
    }

    fun cancelTimer() {
        timer.cancel()
    }
//...
}

```

---


If you want to show a countdown with days hours minutes and seconds

```Kotlin
private lateinit var countDownTimer:CountDownTimer
.
.
.
    fun printDifferenceDateForHours() {

            val currentTime = Calendar.getInstance().time
            val endDateDay = "03/02/2020 21:00:00"
            val format1 = SimpleDateFormat("dd/MM/yyyy hh:mm:ss",Locale.getDefault())
            val endDate = format1.parse(endDateDay)

            //milliseconds
            var different = endDate.time - currentTime.time
            countDownTimer = object : CountDownTimer(different, 1000) {

                override fun onTick(millisUntilFinished: Long) {
                    var diff = millisUntilFinished
                    val secondsInMilli: Long = 1000
                    val minutesInMilli = secondsInMilli * 60
                    val hoursInMilli = minutesInMilli * 60
                    val daysInMilli = hoursInMilli * 24

                    val elapsedDays = diff / daysInMilli
                    diff %= daysInMilli

                    val elapsedHours = diff / hoursInMilli
                    diff %= hoursInMilli

                    val elapsedMinutes = diff / minutesInMilli
                    diff %= minutesInMilli

                    val elapsedSeconds = diff / secondsInMilli

                    txt_timeleft.text = "$elapsedDays days $elapsedHours hs $elapsedMinutes min $elapsedSeconds sec"
                }

                override fun onFinish() {
                    txt_timeleft.text = "done!"
                }
            }.start()
        }
If you are navigating to another activity/fragment, make sure to cancel the countdown

countDown
```


If you are navigating to another activity/fragment, make sure to cancel the countdown


```Kotlin
countDownTimer.cancel()
```


Code output


```
51 days 17 hs 56 min 5 sec
```

---
CountDownTimer in Kotlin:

```Kotlin
object: CountDownTimer(3000, 1000){
    override fun onTick(p0: Long) {}
    override fun onFinish() {
        //add your code here
    }
 }.start()
```

---


```Kotlin

class CustomCountDownTimer(var mutableLiveData: MutableLiveData<String>) {

    lateinit var timer: CountDownTimer
    val zone = ZoneId.systemDefault()
    val startDateTime: ZonedDateTime = LocalDateTime.now().atZone(zone)

    fun start(endOn: Long) {
        if (this::timer.isInitialized) {
            return
        }
        timer = object : CountDownTimer(endOn * 1000, 1000) {

            override fun onTick(millisUntilFinished: Long) {

                val stringBuilder = StringBuilder()

                val endDateTime: ZonedDateTime =
                    Instant.ofEpochMilli(millisUntilFinished).atZone(ZoneId.systemDefault())
                        .toLocalDateTime().atZone(zone)

                var diff: Duration = Duration.between(startDateTime, endDateTime)



                if (diff.isZero() || diff.isNegative) {
                    stringBuilder.append("Already ended!")
                } else {
                    val days: Long = diff.toDays()

                    if (days != 0L) {
                        stringBuilder.append("${days}day : ")
                        diff = diff.minusDays(days)
                    }

                    val hours: Long = diff.toHours()
                    stringBuilder.append("${hours}hr : ")
                    diff = diff.minusHours(hours)

                    val minutes: Long = diff.toMinutes()
                    stringBuilder.append("${minutes}min : ")
                    diff = diff.minusMinutes(minutes)

                    val seconds: Long = diff.getSeconds()

                    stringBuilder.append("${seconds}sec")

                }

                mutableLiveData.postValue(stringBuilder.toString())
                //Log.d("CustomCountDownTimer", stringBuilder.toString())
            }

            override fun onFinish() {
            }
        }

        timer.start()
    }


    fun getTimerState(): LiveData<String> {
        return mutableLiveData
    }
}
```

How to use it:

```kotlin
val liveData: MutableLiveData<String> = MutableLiveData()
val customCountDownTimer = CustomCountDownTimer(liveData)
customCountDownTimer.start(1631638786) //Epoch timestamp
customCountDownTimer.mutableLiveData.observe(this, Observer { counterState ->
            counterState?.let {
                println(counterState)
            }
        })
```

Output:

```
22hr : 42min : 51sec //when less than 4hr are remaining

1day : 23hr : 52min : 44sec // in other cases

Sha
```



