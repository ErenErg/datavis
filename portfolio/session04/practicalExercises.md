---
id: litvis

narrative-schemas:
  - ../../lectures/narrative-schemas/teaching.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

@import "../../lectures/css/datavis.less"

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```

<!-- Everything above this line should probably be left untouched. -->

# Session 4: Practical Exercises

{(task|}

Use this document as a place to add your answers to the week's practical exercises.

{|task)}

## Q1. Matching Visual Variables

I completed both visual variable tables in the Session 4 lecture notes.

I was not able to be in class and couldn't have a group discussion but I found that position and size work quite well for quantitative data, whereas colour hue and shape are more useful for distinguishing categories. Some variables like colour lightness and saturation are also useful for showing order.

## Q2. Graphical Summary of Channel Properties

```elm {l}
vvTable =
    let
        table =
            """visualVariable,q,o,s,a,d
colour hue,1,1,3,3,3
colour saturation,2,3,2,2,2
colour lightness,2,3,2,2,2
orientation,1,2,2,2,2
shape,1,1,3,3,2
texture,1,2,2,2,2
arrangement,2,2,2,2,2
size,3,3,3,2,3
focus,1,2,3,2,3
location,3,3,3,3,2"""
                |> fromCSV
                |> gather "property" "rating"
                    [ ( "q", "quantitative" )
                    , ( "o", "orderable" )
                    , ( "s", "selective" )
                    , ( "a", "associative" )
                    , ( "d", "dissociative" )
                    ]
    in
    dataFromColumns []
        << dataColumn "visualVariable" (table |> strColumn "visualVariable" |> strs)
        << dataColumn "property" (table |> strColumn "property" |> strs)
        << dataColumn "rating" (table |> numColumn "rating" |> nums)
```

```elm {l v}
channelSummary : Spec
channelSummary =
    let
        enc =
            encoding
                << position Y [ pName "visualVariable", pNominal ]
                << position X [ pName "property", pNominal ]
                << color [ mName "rating", mOrdinal ]
    in
    toVegaLite [ width 450, height 350, vvTable [], enc [], rect [] ]
```

### Evaluation

I used a heatmap because I thought it would be a simple way to compare the ratings of the different visual variables.

The colours make it easier to see which variables are stronger or weaker for each property. However, it can be a bit difficult to tell the exact rating just by looking at the colours.

## Q3. Design Challenge: Exploring Data-Channel Options

For this challenge I used the Gapminder dataset containing income, health and population information for different countries.

```elm {l}
gapData : Data
gapData =
    dataFromUrl "https://vega.github.io/vega-lite/data/gapminder-health-income.csv" []
```

### Design 1 – Basic Scatterplot

```elm {l v}
design1 : Spec
design1 =
    let
        enc =
            encoding
                << position X [ pName "income", pQuant ]
                << position Y [ pName "health", pQuant ]
    in
    toVegaLite
        [ width 500
        , height 350
        , gapData
        , enc []
        , circle []
        ]
```

I started with a simple scatterplot comparing income and health. It is fairly easy to understand because the positions of the circles show the values.

However, it does not show the population of each country, so some information is missing.

### Design 2 – Population as Circle Size

```elm {l v}
design2 : Spec
design2 =
    let
        enc =
            encoding
                << position X [ pName "income", pQuant ]
                << position Y [ pName "health", pQuant ]
                << size [ mName "population", mQuant ]
    in
    toVegaLite
        [ width 500
        , height 350
        , gapData
        , enc []
        , circle []
        ]
```

For the second design I added population using circle size.

I think this is an improvement because it shows three variables in one graph. The main disadvantage is that the larger circles sometimes overlap the smaller ones, making it more difficult to see individual countries.

### Design 3 – Double Encoding

```elm {l v}
design3 : Spec
design3 =
    let
        enc =
            encoding
                << position X [ pName "income", pQuant ]
                << position Y [ pName "health", pQuant ]
                << size [ mName "population", mQuant ]
                << color [ mName "income", mQuant ]
    in
    toVegaLite
        [ width 500
        , height 350
        , gapData
        , enc []
        , circle []
        ]
```

In this design I used both position and colour to represent income.

The colour makes the differences in income more noticeable, although the information is already shown on the x-axis. I think this makes the chart a little more complicated than necessary.

### Design 4 – Flannery Scaling

```elm {l v}
design4 : Spec
design4 =
    let
        enc =
            encoding
                << position X [ pName "income", pQuant ]
                << position Y [ pName "health", pQuant ]
                << size
                    [ mName "population"
                    , mQuant
                    , mScale
                        [ scType scPow
                        , scExponent (1 / 0.87)
                        , scRange (raNums [ 0, 5000 ])
                        ]
                    ]
    in
    toVegaLite
        [ width 500
        , height 350
        , gapData
        , enc []
        , circle []
        ]
```

For the fourth design I tried Flannery scaling to change how the circle sizes represent population.

This makes the differences between the smaller and larger circles more noticeable compared with the default scaling. It might help people judge population differences better, but I also think it can make the graph look more crowded.

### Overall Evaluation

Although the designs of the axes came out a bit weird and smaller than I expected. I think Design 2 is the most useful because it shows income, health and population without making the graph too complicated.

Position works well for income and health because they are quantitative variables, while size is a reasonable choice for population.

Double encoding in Design 3 helps make income more noticeable, but I don't think it adds much new information.

Showing everything in one graph is useful for understanding relationships, although it can get cluttered. Using separate graphs might make individual comparisons easier.

Flannery scaling could help with estimating population differences, but I think the default bubble chart is easier to understand overall.
