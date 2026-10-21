---
follows: imdbRoot
---

# Minimalist IMDB Histogram

```elm {l v}
minimalHistogram : Spec
minimalHistogram =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        ]
                    )

        enc =
            encoding
                << position X
                    [ pName "IMDB Rating"
                    , pBin []
                    , pAxis [ axTitle "" ]
                    ]
                << position Y
                    [ pAggregate opCount
                    , pAxis [ axTitle "" ]
                    ]
    in
    toVegaLite
        [ cfg []
        , data
        , enc []
        , bar [ maFillOpacity 0.6 ]
        ]
```

## Evaluation

I removed the colour encoding, axis titles, tick marks and chart border to make the visualization more minimal.

I think removing the colour works well because the IMDB rating is already represented on the x-axis, so the colour was repeating information that was already shown. Removing the border and some axis details also makes the chart look less cluttered.

However, removing too much information can make the chart harder to understand. I kept the basic axis values because they are still needed to understand the ratings and frequency of films.

Overall, I think the minimalist version is easier to read, but some non-data ink is still useful when it helps the viewer understand the graph.
