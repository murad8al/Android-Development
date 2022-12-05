### LiveData

MainActitivyViewModel.kt

```kotlin
import android.os.CountDownTimer
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class MainActitivyViewModel: ViewModel() {

    private lateinit var timer: CountDownTimer

    /** timerValue - LiveData object. We are going to set this value from MainActivity */
    var timerValue = MutableLiveData<Long>()

    /** Finished - LiveData object. This value is being observer from MainActivity
     * Here I'm using a variable to get a private MutableLiveData value */
    private var _finished = MutableLiveData<Boolean>()
    val finished: LiveData<Boolean>
        get() = _finished

    /** Seconds - LiveData object. This value is being observer from MainActivity
     * Here I'm using a function to get a private MutableLiveData value */
    private val _seconds = MutableLiveData<Int>()
    fun seconds(): LiveData<Int>{
        return _seconds
    }

    /** Start Timer. This function is triggered from MainActivity. */
    fun startTimer(){
        timer = object : CountDownTimer(timerValue.value!!.toLong(), 1000){
            override fun onFinish() {
                _finished.value = true
            }

            override fun onTick(p0: Long) {
                val timeLeft = p0/1000
                _seconds.value = timeLeft.toInt()
            }
        }.start()
    }

    /** Stop Timer. This function is triggered from MainActivity. */
    fun stopTimer(){
        timer.cancel()
    }

}import android.os.CountDownTimer
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class MainActitivyViewModel: ViewModel() {

    private lateinit var timer: CountDownTimer

    /** timerValue - LiveData object. We are going to set this value from MainActivity */
    var timerValue = MutableLiveData<Long>()

    /** Finished - LiveData object. This value is being observer from MainActivity
     * Here I'm using a variable to get a private MutableLiveData value */
    private var _finished = MutableLiveData<Boolean>()
    val finished: LiveData<Boolean>
        get() = _finished

    /** Seconds - LiveData object. This value is being observer from MainActivity
     * Here I'm using a function to get a private MutableLiveData value */
    private val _seconds = MutableLiveData<Int>()
    fun seconds(): LiveData<Int>{
        return _seconds
    }

    /** Start Timer. This function is triggered from MainActivity. */
    fun startTimer(){
        timer = object : CountDownTimer(timerValue.value!!.toLong(), 1000){
            override fun onFinish() {
                _finished.value = true
            }

            override fun onTick(p0: Long) {
                val timeLeft = p0/1000
                _seconds.value = timeLeft.toInt()
            }
        }.start()
    }

    /** Stop Timer. This function is triggered from MainActivity. */
    fun stopTimer(){
        timer.cancel()
    }

}
```

MainActivity.kt

```kotlin
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Toast
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModelProvider
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val viewModel = ViewModelProvider(this).get(MainActitivyViewModel::class.java)

        /** Observing seconds() LiveData object from ViewModel */
        viewModel.seconds().observe(this, Observer {
            number_txt.text = it.toString()
        })
        /** Observing finished LiveData object from ViewModel */
        viewModel.finished.observe(this, Observer {
            if(it){
                Toast.makeText(this, "Finished!", Toast.LENGTH_SHORT).show()
            }
        })

        start_btn.setOnClickListener {
            if(number_input.text.isEmpty() || number_input.text.length < 4){
                Toast.makeText(this, "Invalid Number", Toast.LENGTH_SHORT).show()
            }else{
                viewModel.timerValue.value = number_input.text.toString().toLong()
                viewModel.startTimer()
            }

        }
        stop_btn.setOnClickListener {
            number_txt.text = "0"
            viewModel.stopTimer()
        }

    }
}
```



[Video](https://youtu.be/suC0OM5gGAA)