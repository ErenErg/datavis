---
follows: lifeExpectancyData
---

# Life Expectancy Visualization

I wanted to see how life expectancy differs between countries in 2004.

```elm {l v}
lifeHistogram : Spec
lifeHistogram =
    let
        trans =
            transform
                << filter (fiExpr "datum.time == 2004")

        enc =
            encoding
                << position X
                    [ pName "life_expectancy_at_birth"
                    , pQuant
                    , pBin []
                    ]
                << position Y
                    [ pAggregate opCount
                    , pQuant
                    ]
    in
    toVegaLite [ width 550, lifeData, trans [], enc [], bar [] ]
```

## Evaluation

I used a histogram to show the distribution of life expectancy across different countries in 2004.

I chose this because I wanted to see whether most countries have similar life expectancies or whether there are big differences.

One limitation is that the chart does not show which countries have the highest or lowest life expectancy, so a map could be useful as another design.
