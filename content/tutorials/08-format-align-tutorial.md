# Format & align tutorial

## Number format tutorial

[Sketch](https://beta.data2viz.io/yevhenii.zapletin/sketches/xXYMQY)

```
import io.data2viz.color.*
import io.data2viz.scale.*
import io.data2viz.math.*
import io.data2viz.geom.*
import io.data2viz.viz.*
import io.data2viz.format.*

fun main() {
    val vizWidth = 600.0
    val vizHeight = 100.0
    val margin = 40.0

    val startNumber = 0
    val endNumber = 1000000000

    val numbers = IntProgression.fromClosedRange(startNumber, endNumber, 100000000)

    val tickStep = 1000000

    val scale = Scales.Continuous.linear {
        domain = listOf(startNumber.toDouble(), endNumber.toDouble())
        range = listOf(margin, vizWidth - margin)
    }

    var counter = 0
    viz {


        size = size(vizWidth, vizHeight)  //<- you need a viz size

        line {
            x1 = margin
            x2 = vizWidth - margin
            y1 = vizHeight / 2
            y2 = vizHeight / 2
        }

        val tickLine = line {
            strokeWidth = 2.0
            stroke = Colors.Web.black
            y1 = vizHeight * 0.5
            y2 = vizHeight * 0.6
        }
        val tickText = text {
            y = vizHeight * 0.7
            strokeWidth = 2.0
            fill = Colors.Web.black
            fontSize = 10.0
            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
        }

        numbers.forEach {
            line {
                x1 = scale(it)
                y1 = vizHeight * 0.4
                x2 = scale(it)
                y2 = vizHeight * 0.5
                strokeWidth = 2.0
                stroke = Colors.Web.black
            }
            text {
                x = scale(it)
                y = vizHeight * 0.3
                fill = Colors.Web.black
                fontSize = 10.0
                textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                textContent = "${formatter(".2e")(it.toDouble())}"
            }
        }
        animation {
            counter = (counter + tickStep) % endNumber
            val currentX = scale(counter)
            tickLine.x1 = currentX
            tickLine.x2 = currentX
            tickText.x = currentX
            tickText.textContent = "${formatter(",d")(counter.toDouble())}"
        }
    }.bindRendererOnNewCanvas()
}
```

## Number format table

[Sketch](https://beta.data2viz.io/yevhenii.zapletin/sketches/oPLzrL/)

```
import io.data2viz.color.*
import io.data2viz.scale.*
import io.data2viz.math.*
import io.data2viz.geom.*
import io.data2viz.viz.*
import io.data2viz.format.*

fun main() {
    val startNumbers = listOf(0.0, 0.00001, 100000.0)
    val steps = listOf(1.0, 0.00005, 10000.0)

    val formats = listOf(
            FormatSpec(),
            FormatSpec(type = Type.DECIMAL, precision = 2),
            FormatSpec(type = Type.DECIMAL_ROUNDED, precision = 2, groupSeparation = true),
            FormatSpec(type = Type.DECIMAL_WITH_SI, precision = 2),
            FormatSpec(type = Type.EXPONENT, precision = 2, align = Align.LEFT, fill = '~', width = 8),
            FormatSpec(type = Type.DECIMAL_OR_EXPONENT, precision = 2, align = Align.RIGHT_WITHOUT_SIGN, fill = '.', width = 8),
            FormatSpec(type = Type.FIXED_POINT, precision = 2),
            FormatSpec(type = Type.PERCENT, precision = 2),
            FormatSpec(type = Type.PERCENT_ROUNDED, precision = 2),
            FormatSpec(type = Type.BINARY, precision = 2),
            FormatSpec(type = Type.OCTAL, precision = 2),
            FormatSpec(type = Type.HEX_LOWERCASE, precision = 2),
            FormatSpec(type = Type.HEX_UPPERCASE, precision = 2)

    )

    val cellWidth = 140.0
    val cellHeight = 50.0

    var secondsCounter = 0

    // this scale map names of the days (as String) to colors
    val scale = ScalesChromatic.Discrete.category10<Double> { domain = startNumbers }
    val startXOffset = cellWidth / 2
    val startYOffset = 25.0

    val tableRows = formats.size + 1
    val tableColumns = startNumbers.size + 1

    val vizWidth = (startNumbers.size + 1) * cellWidth
    val vizHeight = (formats.size + 1) * cellHeight
    val viz = viz {
        size = size(vizWidth, vizHeight)

        text {
            x = startXOffset
            y = startYOffset
            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
            textContent = "Format"
        }

        for (i in 0..tableRows) {
            line {
                x1 = 0.0
                x2 = vizWidth
                y1 = i * cellHeight - 1
                y2 = i * cellHeight - 1
            }
        }

        for (i in 0..tableColumns) {
            line {
                x1 = i * cellWidth - 1
                x2 = i * cellWidth - 1
                y1 = 0.0
                y2 = vizHeight
            }
        }


        line {
            x1 = vizWidth - 1
            x2 = vizWidth - 1
            y1 = 0.0
            y2 = vizHeight
        }


        startNumbers.forEachIndexed { numberIndex, startNumber ->
            text {
                x = startXOffset + (numberIndex + 1) * cellWidth
                y = startYOffset
                textColor = scale(startNumber)
                textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                textContent = "Number"
            }


        }


        val columns =
                startNumbers.mapIndexed { numberIndex, number ->
                    formats.mapIndexed { formatIndex, formatSpec ->
                        text {
                            x = startXOffset
                            y = startYOffset + (formatIndex + 1) * cellHeight
                            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                            textContent = "$formatSpec"
                        }
                        text {
                            x = startXOffset + (numberIndex + 1) * cellWidth
                            y = startYOffset + (formatIndex + 1) * cellHeight
                            textColor = scale(number)
                            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                            textContent = formatter(formatSpec.toString())(number)
                        }
                    }
                }


        animation { timerInMs: Double ->
            // update one time per second
            if (timerInMs / 1000 > secondsCounter) {
                secondsCounter++
                startNumbers.forEachIndexed { index, startNumber ->
                    columns[index].forEachIndexed { rowIndex, text ->
                        text.textContent = formatter(formats[rowIndex].toString())(startNumber + steps[index] * secondsCounter)
                    }
                }
            }
        }
    }.bindRendererOnNewCanvas()
}
```
## Time format table
[Sketch](https://beta.data2viz.io/yevhenii.zapletin/sketches/ypLAxg/)

```
import io.data2viz.color.*
import io.data2viz.geom.*
import io.data2viz.math.*
import io.data2viz.scale.*
import io.data2viz.viz.*
import io.data2viz.time.*
import io.data2viz.timeFormat.*

fun main() {
	val startDates = listOf(Date(), Date(), Date(), Date())
    val steps = listOf(timeMillisecond, timeHour, timeDay, timeMonth)

    val formats = listOf(
            "%L", // milliseconds
            "%H:%M:%S",
            "%d:%m:%Y",
            "%x, %X",
            "%-m/%-d/%Y",
            "%-I:%M:%S %p",
            "%a", // Weekday
            "%B", // Month
            "%Z" // Month
    )

    val cellWidth = 140.0
    val cellHeight = 50.0

    var secondsCounter = 0

    // this scale map names of the days (as String) to colors
    val scale = ScalesChromatic.Discrete.category10<Date> { domain = startDates }
    val startXOffset = cellWidth / 2
    val startYOffset = 25.0

    val tableRows = formats.size + 1
    val tableColumns = startDates.size + 1

    val vizWidth = (startDates.size + 1) * cellWidth
    val vizHeight = (formats.size + 1) * cellHeight
    val viz = viz {
        size = size(vizWidth, vizHeight)

        text {
            x = startXOffset
            y = startYOffset
            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
            textContent = "Format"
        }

        for (i in 0..tableRows) {
            line {
                x1 = 0.0
                x2 = vizWidth
                y1 = i * cellHeight - 1
                y2 = i * cellHeight - 1
            }
        }

        for (i in 0..tableColumns) {
            line {
                x1 = i * cellWidth - 1
                x2 = i * cellWidth - 1
                y1 = 0.0
                y2 = vizHeight
            }
        }


        line {
            x1 = vizWidth - 1
            x2 = vizWidth - 1
            y1 = 0.0
            y2 = vizHeight
        }


        startDates.forEachIndexed { dateIndex, startDate ->
            text {
                x = startXOffset + (dateIndex + 1) * cellWidth
                y = startYOffset
                textColor = scale(startDate)
                textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                textContent = "Date"
            }


        }


        val columns =
                startDates.mapIndexed { dateIndex, date ->
                    formats.mapIndexed { formatIndex, formatSpec ->
                        text {
                            x = startXOffset
                            y = startYOffset + (formatIndex + 1) * cellHeight
                            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                            textContent = "$formatSpec"
                        }
                        text {
                            x = startXOffset + (dateIndex + 1) * cellWidth
                            y = startYOffset + (formatIndex + 1) * cellHeight
                            textColor = scale(date)
                            textAlign = textAlign(TextHAlign.MIDDLE, TextVAlign.MIDDLE)
                            textContent = format(formatSpec)(date)
                        }
                    }
                }


        animation { timerInMs: Double ->
            // update one time per second
            if (timerInMs / 1000 > secondsCounter) {
                secondsCounter++
                startDates.forEachIndexed { index, startDate ->
                    columns[index].forEachIndexed { rowIndex, text ->
                        text.textContent = format(formats[rowIndex])(steps[index].offset(startDate, secondsCounter.toLong()))
                    }
                }
            }
        }
    }.bindRendererOnNewCanvas()
}
```