# Format & align tutorial

## Number format


```
import io.data2viz.color.*
import io.data2viz.scale.*
import io.data2viz.math.*
import io.data2viz.geom.*
import io.data2viz.viz.*
import io.data2viz.format.*

fun main() {
    val fontSize = 80.0
   val formatLeftTop = formatter(type = Type.DECIMAL)
        val formatRightTop = formatter(type = Type.EXPONENT, precision = 6)
        val formatLeftBottom = formatter(type = Type.DECIMAL_ROUNDED, align = Align.RIGHT_WITHOUT_SIGN, width = 8, fill = '.', zero = false)
        val formatRightBottom = formatter(type = Type.HEX_LOWERCASE)
        val viz = viz {
            size = size(fontSize * 2, fontSize * 2)

            var count = .0
            group {

                val leftTop = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.LEFT
                    vAlign = TextVAlign.BASELINE
                    textColor = Colors.Web.black
                }
                val rightTop = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.RIGHT
                    vAlign = TextVAlign.BASELINE
                    textColor = Colors.Web.blue
                }

                val leftBottom = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.LEFT
                    vAlign = TextVAlign.HANGING
                    textColor = Colors.Web.green
                }

                val rightBottom = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.RIGHT
                    vAlign = TextVAlign.HANGING
                    textColor = Colors.Web.green
                }
                animation {
                    count += 0.2
                    leftTop.textContent = " ${formatLeftTop(count)} "
                    leftBottom.textContent = " ${formatLeftBottom(count)} "
                    rightTop.textContent = " ${formatRightTop(count)} "
                    rightBottom.textContent = " ${formatRightBottom(count)} "
                    
                }
            }
        }
        .bindRendererOnNewCanvas()
}
```

## Time format

```
import io.data2viz.color.*
import io.data2viz.scale.*
import io.data2viz.math.*
import io.data2viz.geom.*
import io.data2viz.viz.*
import io.data2viz.timeFormat.*
import io.data2viz.time.*

fun main() {
   val fontSize = 80.0

        val formatLeftTop = format("%H:%S:%M")
        val formatRightTop = format("%A")
        val formatLeftBottom = format("%Y.%M.%d")
        val formatRightBottom = format("%L")



        val viz = viz {
            size = size(fontSize * 2, fontSize * 2)

            var date = Date()
            group {

                val leftTop = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.LEFT
                    vAlign = TextVAlign.BASELINE
                    textColor = Colors.Web.black
                }
                val rightTop = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.RIGHT
                    vAlign = TextVAlign.BASELINE
                    textColor = Colors.Web.blue
                }

                val leftBottom = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.LEFT
                    vAlign = TextVAlign.HANGING
                    textColor = Colors.Web.green
                }

                val rightBottom = text {
                    x = fontSize
                    y = fontSize
                    hAlign = TextHAlign.RIGHT
                    vAlign = TextVAlign.HANGING
                    textColor = Colors.Web.green
                }
                animation {
					date = timeHour.offset(date, 4)
                    date = timeMillisecond.offset(date, 10)
                    leftTop.textContent = " ${formatLeftTop(date)}"
                    leftBottom.textContent = " ${formatLeftBottom(date)} "
                    rightTop.textContent = " ${formatRightTop(date)} "
                    rightBottom.textContent = " ${formatRightBottom(date)} "

                }
            }
        }
        .bindRendererOnNewCanvas()
}
```