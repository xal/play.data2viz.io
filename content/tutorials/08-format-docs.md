# Format text & time



## Text hAlign & vAlign

`io.data2viz.viz.TextNode` contains `hAlign` and `vAlign` properties which can be used for **align text related to (x,y) coordinates pivot**.

| Propoerty  | Class | Values |
|---|---|---|
| `hAlign` | `TextHAlign` | `LEFT` `MIDDLE` `RIGHT`  |
| `vAlign` | `TextVAlign` | `HANGING` `MIDDLE` `BASELINE`  |

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

Package `io.data2viz.time` provides functions for operations with date time. For example how many days between two dates

`Day().offset(Date(), 5)` will return current date + 5 days

Possible intervals: `Day` `Month` ...

Each interval have default created object `val timeDay = Day()` You can use it `timeDay.offset(Date(), 5)`

## Interval functions

| Function  | Result | Description | 
|---|---|---|
| `floor(date: Date)`  | `Date` |  Returns a new date representing the latest interval boundary date before or equal to date |  |
| `ceil(date: Date)`  | `Date` |  Returns a new date representing the earliest interval boundary date after or equal to date |  |
| `round(date: Date)`  | `Date` |  Returns a new date representing the closest interval boundary date to date. |  |
| `offset(date: Date, step: Long = 1)`  | `Date` |  Returns a new date equal to date plus step intervals |  |
| `range(start: Date, stop: Date, step: Long = 1)`  | `List<Date>` |   |  |
| `filter(test: (Date) -> Boolean)`  | `Interval` |   |  |
| `every(step: Int)`  | `Interval` |   |  |
| `count(start: Date, stop: Date)`  | `Int` |   |  |


## Examples

# Format numbers

Package `io.data2viz.format` help you format numbers to string


## Formatter

You can create `io.data2viz.format.formatter` object `val format = formatter(type = Type.DECIMAL_ROUNDED)`

And use it to format numbers `format(81.5)` will return `"82"`

It also possible to use `io.data2viz.format.formatter` with format string, for example
`val format = formatter("d")` instead of `val format = formatter(type = Type.DECIMAL_ROUNDED)`

Form

 `[​[fill]align][sign][symbol][0][width][,][.exponent][type]`

| Argument  | Type | Default Value | Description |
|---|---|---|---|
| `fill`  | `Char` | `' '` | The char used to fill the spaces (when width is larger than the representation) | 
| `align `  | `Align ` | `Align.RIGTH` | The alignment of the number inside the available space | 
| `sign `  | `Sign` | `Sign.MINUS` | Defines how positive and negative numbers are represented. | 
| `symbol `  | `Symbol?` | `null` | Adds `[Symbol.CURRENCY]` or `[Symbol.NUMBER_BASE]` to the number representation. | 
| `zero `  | `Boolean` | `false` | This option enables zero-padding; this implicitly sets fill to `0` and aligne to `[Align.RIGHT_WITHOUT_SIGN]`  | 
| `width `  | `Int?` | `null` | The width defines the minimum field width; if not specified, then the width will be determined by the content | 
| `groupSeparation `  | `Boolean` | `false` | This option enables the use of a group separator, such as a comma for thousands. | 
| `precision `  | `Int?` | `null` | on the type, the precision either indicates the number of digits that follow the decimal point (types f and %), or the number of significant digits (types ​, e, g, r, s and p). If the precision is not specified, it defaults to 6 for all types except ​ (none), which defaults to 12. Precision is ignored for integer formats (types b, o, d, x, X and c). See precisionFixed and precisionRound for help picking an appropriate precision. | 
| `type `  | `Type ` | `Type.NONE` |  The type of number representation | 



### Type

| Type  | Value | Description | 
|---|---|---|
| `NONE` |  | like `DECIMAL_OR_EXPONENT`, but trim insignificant trailing zeros. |  |
| `DECIMAL` | `r` | decimal notation, rounded to significant digits.  |
| `DECIMAL_ROUNDED` | `d` | decimal notation, rounded to integer | | |
| `DECIMAL_WITH_SI` | `s` | decimal notation with an `SI prefix ``Locale.formatPrefix`, rounded to significant digits.  | | |
| `DECIMAL_OR_EXPONENT` | `g` | either decimal or exponent notation, rounded to significant digits. | | |
| `EXPONENT` | `e` | Exponent notation | | |
| `FIXED_POINT` | `f` | Fixed point notation | | |
| `PERCENT` | `%` | multiply by 100, and then decimal notation with a percent sign. | | |
| `PERCENT_ROUNDED` | `p` | multiply by 100, round to significant digits, and then decimal notation with a percent sign | | |
| `BINARY` | `b` | binary notation, rounded to integer | | |
| `OCTAL` | `o` | octal notation, rounded to integer | | |
| `HEX_LOWERCASE` | `x` | hexadecimal notation, using lower-case letters, rounded to integer | | |
| `HEX_UPPERCASE` | `X` | hexadecimal notation, using upper-case letters, rounded to integer | | |


### Align

| Align  | Value | Description | 
|---|---|---|
| `RIGHT`  | `>` | Forces the field to be right-aligned within the available space. (Default behavior). |  | 
| `LEFT`  | `<` | Forces the field to be left-aligned within the available space. |  | 
| `CENTER`  | `^` | Forces the field to be centered within the available space |  | 
| `RIGHT_WITHOUT_SIGN`  | `=` | `like [RIGTH], but with any sign and symbol to the left of any padding` |  | 


### Locale

  var decimalSeparator: String = ".",
        var grouping: List<Int> = listOf(3),
        var groupSeparator: String = ",",
        var currency: List<String> = listOf("$", ""),
        var numerals: Array<String>? = null,
        var percent: String = "%"

### Examples

| Format  | Number value | String result | 
|---|---|---|
| `formatter()`  | `80.0` | `80` | 
| `formatter()`  | `0.00005` | `5e-5` | 
| `formatter("r")`  | `80.0` | `80.0000` | 
| `formatter()`  | `80.0` | `80` | 
| `formatter()`  | `80.0` | `80` | 



## Prefix

`Locale.formatPrefix`

| Prefix  | Name | Value |
|---|---|---|
| y | yocto | 10⁻²⁴ |
| z | zepto | 10⁻²¹ |
| a | atto | 10⁻¹⁸ |
| f | femto | 10⁻¹⁵ |
| p | pico | 10⁻¹² |
| n | nano | 10⁻⁹ |
| µ | micro | 10⁻⁶ |
| m | milli | 10⁻³ |
|  | (none) | 10⁻⁰ |
| k | kilo | 10³ |
| M | mega | 10⁶ |
| G | giga | 10⁹ |
| T | tera | 10¹² |
| P | peta | 10¹⁵ |
| E | exa | 10¹⁸ |
| Z | zetta | 10¹⁵ |
| Y | yotta | 10²⁴ |

### Examples

# Format time

Package `io.data2viz.timeFormat` help you format time

`io.data2viz.timeFormat.format(specifier: String)`
`io.data2viz.timeFormat.parse(specifier: String)`

| Specifier  | Name | Example result |
|---|---|---|
| A | formatShortWeekday | Thu |
 
    
## Locale

## Examples

| Specifiers  |  Result |
|---|---|
| `H:M:S` | `17:20:37`  |