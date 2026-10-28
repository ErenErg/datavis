---
id: litvis

elm:
  dependencies:
    gicentre/elm-vegalite: latest
---

```elm {l=hidden}
import VegaLite exposing (..)
```

# Life Expectancy Data

This dataset is from Gapminder and shows life expectancy at birth for different countries over time.

```elm {l}
lifeData : Data
lifeData =
    dataFromUrl "https://raw.githubusercontent.com/open-numbers/ddf--gapminder--life_expectancy/master/ddf--datapoints--life_expectancy_at_birth--by--geo--time.csv" []
```
