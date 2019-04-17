# Format text & time



## Text hAlign & vAlign

`io.data2viz.viz.TextNode` contains `hAlign` and `vAlign` properties which can be used for **align text related to (x,y) coordinates pivot**.

| Propoerty  | Class | Values |
|---|---|---|
| hAlign | TextHAlign | LEFT, MIDDLE, RIGHT  |
| vAlign | TextVAlign | HANGING, MIDDLE, BASELINE  |
## hAlign Example

```height=100
import io.data2viz.color.*
import io.data2viz.geom.*
import io.data2viz.math.*
import io.data2viz.viz.*

fun main() {
    viz {
        size = size(600, 600)

        val defaultSize = 80.0
        val defaultX = 240.0
        val defaultY = 80.0
        text {
            x = defaultX
            y = defaultY
            hAlign = TextHAlign.LEFT
            textContent = "Left"
            fontSize = defaultSize
            textColor = Colors.Web.green
        }
        text {
            x = defaultX
            y = defaultY
            hAlign = TextHAlign.RIGHT
            textContent = "Right"
            fontSize = defaultSize
            textColor = Colors.Web.red
        }
         text {
            x = defaultX
            y = defaultY * 2
            hAlign = TextHAlign.MIDDLE
            textContent = "Middle"
            fontSize = defaultSize
            textColor = Colors.Web.blue
        }
        // to here
    }.bindRendererOnNewCanvas()
}
```

## vAlign Example

```height=100
import io.data2viz.color.*
import io.data2viz.geom.*
import io.data2viz.math.*
import io.data2viz.viz.*

fun main() {
    viz {
        size = size(600, 600)

        val defaultSize = 20.0
        val defaultX = 80.0
        val defaultY = 20.0
        text {
            x = defaultX
            y = defaultY
            vAlign = TextVAlign.HANGING
            textContent = "Hanging"
            fontSize = defaultSize
            textColor = Colors.Web.green
        }
        text {
            x = defaultX*2
            y = defaultY
            vAlign = TextVAlign.MIDDLE
            textContent = "Middle"
            fontSize = defaultSize
            textColor = Colors.Web.red
        }
         text {
            x = defaultX*3
            y = defaultY
            vAlign = TextVAlign.BASELINE
            textContent = "Baseline"
            fontSize = defaultSize
            textColor = Colors.Web.blue
        }
        // to here
    }.bindRendererOnNewCanvas()
}
```


# Time

Package `io.data2viz.time` provides functions for operations with time

# Format numbers

Package `io.data2viz.format` help you format numbers


# Format time

Package `io.data2viz.timeFormat` help you format time