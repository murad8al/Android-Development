**1. Using `compareTo()` function**

The standard solution to compare two Date objects is by using the compareTo() function. It returns a value

1. = 0, if both dates are equal.
2. < 0, if date is before the specified date.
3. `> 0`, if date is after the specified date.

The following program demonstrates it:


```kotlin
import java.text.SimpleDateFormat
import java.util.*
 
fun main() {
    val d1 = "12/24/2018"
    val d2 = "10/10/2017"
 
    val sdf = SimpleDateFormat("MM/dd/yyyy")
 
    val firstDate: Date = sdf.parse(d1)
    val secondDate: Date = sdf.parse(d2)
 
    val cmp = firstDate.compareTo(secondDate)
    when {
        cmp > 0 -> {
            System.out.printf("%s is after %s", d1, d2)
        }
        cmp < 0 -> {
            System.out.printf("%s is before %s", d1, d2)
        }
        else -> {
            print("Both dates are equal")
        }
    }
}

```

**Output:**

```
12/24/2018 is after 10/10/2017

```

**2. Using before(), after(), and equals() function**

Alternatively, you can also use the before(), after() and equals() from Date class, which returns a boolean value based on the comparison result.

1.after() returns true if the date is after the specified date.

2.before() returns true if the date is before the specified date.

3.equals() returns true if both dates are equal.

The following example demonstrates the usage of these methods to compare two LocalDate:

```kotlin
import java.text.SimpleDateFormat
import java.util.*
 
fun main() {
    val d1 = "12/24/2018"
    val d2 = "10/10/2017"
 
    val sdf = SimpleDateFormat("MM/dd/yyyy")
 
    val firstDate: Date = sdf.parse(d1)
    val secondDate: Date = sdf.parse(d2)
 
    when {
        firstDate.after(secondDate) -> {
            System.out.printf("%s is after %s", d1, d2)
        }
        firstDate.before(secondDate) -> {
            System.out.printf("%s is before %s", d1, d2)
        }
        firstDate == secondDate -> {
            print("Both dates are equal")
        }
    }
}

```

**Output:**

```
12/24/2018 is after 10/10/2017

```

**3. Using isBefore(), isAfter(), isEqual() functions**

To compare LocalDate, LocalTime, or LocalDateTime objects, you can use the isBefore(), isAfter(), isEqual() functions. The following example demonstrates the usage of these methods to compare two LocalDate:

```kotlin
import java.time.LocalDate
import java.time.format.DateTimeFormatter
 
fun main() {
    val d1 = "12/24/2018"
    val d2 = "10/10/2017"
 
    val formatter = DateTimeFormatter.ofPattern("MM/dd/yyyy")
 
    val firstDate: LocalDate = LocalDate.parse(d1, formatter)
    val secondDate: LocalDate = LocalDate.parse(d2, formatter)
 
    when {
        firstDate.isAfter(secondDate) -> {
            System.out.printf("%s is after %s", d1, d2)
        }
        firstDate.isBefore(secondDate) -> {
            System.out.printf("%s is before %s", d1, d2)
        }
        firstDate.isEqual(secondDate) -> {
            print("Both dates are equal")
        }
    }
}
```

**Output:**

```
12/24/2018 is after 10/10/2017

```

# How to compare date Time to date in kotlin Android

When I was trying to compare date time with date . My date time was in the string and on that time in comparing I was getting error then I change date time String to LocalDateTime Format after that I tried to compare then it is working fine.

**We will compare today date to given date Time(2021–01–21T17:16)**

```kotlin

val calender = Calendar.getInstance()

val year = calender.get(Calendar.YEAR)
val month = calender.get(Calendar.MONTH) + 1
val day = calender.get(Calendar.DAY_OF_MONTH)
val todayDate = "$year-0$month-$day"


val convertedTodayDate = LocalDate.parse(todayDate)

val convertedDate = LocalDateTime.parse("2021-01-21T17:16")
val date = convertedDate.toLocalDate()
val dateFormatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.MEDIUM)
val formattedExpiryDate = date.format(dateFormatter)
val formattedTodayDate = convertedTodayDate.format(dateFormatter)


if (formattedExpiryDate?.compareTo(formattedTodayDate)!! >0){
    Toast.makeText(this, "Today date is less than given date", Toast.LENGTH_SHORT).show()

}else if(formattedExpiryDate?.compareTo(formattedTodayDate)!! <0){
       Toast.makeText(this, "Today date is gretter than given date", Toast.LENGTH_SHORT).show()

}else if(formattedExpiryDate?.compareTo(formattedTodayDate) ==0){
        Toast.makeText(this, "Both date is same or date is matched", Toast.LENGTH_SHORT).show()
}
```


