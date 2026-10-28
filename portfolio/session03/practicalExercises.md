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

# Session 3: Practical Exercises

{(task|}

Use this document as a place to add your answers to the week's practical exercises.

{|task)}

## Q1. Tidying Data Tables

### (a) UK General Election Results 2019

This table is already tidy. Each row represents a political party and the columns contain separate variables for vote percentage and number of MPs.

### (b) Tokyo 2021 Paralympic Medal Table

This table is not tidy because the medal types are stored as separate columns. I would change it so that the medal type becomes its own variable.

| Rank | Country | MedalType | Number |
| ---- | ------- | --------- | ------ |
| 1    | China   | Gold      | 96     |
| 1    | China   | Silver    | 60     |
| 1    | China   | Bronze    | 51     |
| 2    | GB      | Gold      | 41     |
| 2    | GB      | Silver    | 38     |
| 2    | GB      | Bronze    | 45     |
| 3    | US      | Gold      | 37     |
| 3    | US      | Silver    | 36     |
| 3    | US      | Bronze    | 31     |
| 4    | RPC     | Gold      | 36     |
| 4    | RPC     | Silver    | 33     |
| 4    | RPC     | Bronze    | 49     |

### (c) Mexico Weather Station

This table is not tidy because maximum and minimum temperatures are stored as separate columns. These can instead be represented by temperature type and temperature value.

| id      | date       | TemperatureType | Temperature |
| ------- | ---------- | --------------- | ----------- |
| MX17004 | 2010-01-30 | Max             | 27.8        |
| MX17004 | 2010-01-30 | Min             | 14.5        |
| MX17004 | 2010-02-02 | Max             | 27.3        |
| MX17004 | 2010-02-02 | Min             | 14.4        |
| MX17004 | 2010-02-03 | Max             | 24.1        |
| MX17004 | 2010-02-03 | Min             | 14.4        |
| MX17004 | 2010-02-11 | Max             | 29.7        |
| MX17004 | 2010-02-11 | Min             | 13.4        |
| MX17004 | 2010-02-23 | Max             | 29.9        |
| MX17004 | 2010-02-23 | Min             | 10.7        |
| MX17004 | 2010-03-05 | Max             | 32.1        |
| MX17004 | 2010-03-05 | Min             | 14.2        |
| MX17004 | 2010-03-10 | Max             | 34.5        |
| MX17004 | 2010-03-10 | Min             | 16.8        |
| MX17004 | 2010-03-16 | Max             | 31.1        |
| MX17004 | 2010-03-16 | Min             | 17.6        |
| MX17004 | 2010-04-27 | Max             | 36.3        |
| MX17004 | 2010-04-27 | Min             | 16.7        |
| MX17004 | 2010-05-27 | Max             | 33.2        |
| MX17004 | 2010-05-27 | Min             | 18.2        |

### (d) UK General Election Results 2017 and 2019

This table is not tidy because the years are included in the column names. I would instead add a Year column so each party has a separate observation for each election.

| Party                     | Year | percentVote | numMPs |
| ------------------------- | ---- | ----------- | ------ |
| Conservative              | 2017 | 42.3        | 317    |
| Conservative              | 2019 | 43.6        | 365    |
| Labour                    | 2017 | 40.0        | 262    |
| Labour                    | 2019 | 32.2        | 202    |
| Scottish National Party   | 2017 | 3.0         | 35     |
| Scottish National Party   | 2019 | 3.9         | 48     |
| Liberal Democrats         | 2017 | 7.4         | 12     |
| Liberal Democrats         | 2019 | 11.6        | 11     |
| Democratic Unionist Party | 2017 | 0.9         | 10     |
| Democratic Unionist Party | 2019 | 0.8         | 8      |

## Q2. Tidying Tables Programmatically

### (b) Paralympic Medal Table

```elm {l}
medalsTable : Table
medalsTable =
    """Rank,Country,NumGold,NumSilver,NumBronze
1,China,96,60,51
2,GB,41,38,45
3,US,37,36,31
4,RPC,36,33,49"""
        |> fromCSV

tidyMedals : Table
tidyMedals =
    medalsTable
        |> gather "MedalType" "Number"
            [ ("NumGold", "Gold")
            , ("NumSilver", "Silver")
            , ("NumBronze", "Bronze")
            ]
```

```elm {m}
showMedals : List String
showMedals =
    tableSummary 12 tidyMedals
```

### (c) Mexico Temperature Readings

```elm {l}
weatherTable : Table
weatherTable =
    """id,date,maxTemperature,minTemperature
MX17004,2010-01-30,27.8,14.5
MX17004,2010-02-02,27.3,14.4
MX17004,2010-02-03,24.1,14.4
MX17004,2010-02-11,29.7,13.4
MX17004,2010-02-23,29.9,10.7
MX17004,2010-03-05,32.1,14.2
MX17004,2010-03-10,34.5,16.8
MX17004,2010-03-16,31.1,17.6
MX17004,2010-04-27,36.3,16.7
MX17004,2010-05-27,33.2,18.2"""
        |> fromCSV

tidyWeather : Table
tidyWeather =
    weatherTable
        |> gather "TemperatureType" "Temperature"
            [ ("maxTemperature", "Max")
            , ("minTemperature", "Min")
            ]
```

```elm {m}
showWeather : List String
showWeather =
    tableSummary 20 tidyWeather
```

### (d) UK General Election Results 2017 and 2019

```elm {l}
electionTable : Table
electionTable =
    """Party,percentVote2017,numMPs2017,percentVote2019,numMPs2019
Conservative,42.3,317,43.6,365
Labour,40.0,262,32.2,202
Scottish National Party,3.0,35,3.9,48
Liberal Democrats,7.4,12,11.6,11
Democratic Unionist Party,0.9,10,0.8,8"""
        |> fromCSV

tidyElection : Table
tidyElection =
    electionTable
        |> gather "measureYear" "value"
            [ ("percentVote2017", "percentVote2017")
            , ("numMPs2017", "numMPs2017")
            , ("percentVote2019", "percentVote2019")
            , ("numMPs2019", "numMPs2019")
            ]
        |> disaggregate "measureYear"
            "(percentVote|numMPs)(2017|2019)"
            [ "measure", "Year" ]
        |> spread "measure" "value"
```

```elm {m}
showElection : List String
showElection =
    tableSummary 10 tidyElection
```

The gather function moves values from multiple columns into two new columns. This makes it easier to work with values such as medal type, temperature type and election year.

## Q3. Data Challenge 1: Global Development Literate Data and Visualization

Done and added to session03 portfoilo as "lifeExpectancyData.md" and "lifeExpectancyVis.md".

## Q4. Data Challenge 2: Titanic Survival

I wanted to see how survival on the Titanic differed between men and women depending on their ticket class.

```elm {l v}
titanicSurvivalChart : Spec
titanicSurvivalChart =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json" []

        enc =
            encoding
                << position X
                    [ pName "pclass"
                    , pOrdinal
                    , pTitle "Ticket Class"
                    ]
                << position Y
                    [ pAggregate opCount
                    , pStack stNormalize
                    , pAxis [ axFormat "%" ]
                    , pTitle "Percentage of Passengers"
                    ]
                << color
                    [ mName "survived"
                    , mNominal
                    , mTitle "Survived (1 = Yes, 0 = No)"
                    ]
                << column [ fName "sex", fHeader [ hdTitle "Gender" ] ]
    in
    toVegaLite [ width 220, data, enc [], bar [] ]
```

### Explanation

I used a stacked bar chart to compare the percentage of passengers who survived or died depending on their ticket class and gender.

I chose percentages instead of just counting passengers because each group had different numbers of people. This makes comparing their chances of survival a bit easier.

### What I Observed

The chart suggests that women generally had higher survival rates than men. First-class passengers also appear to have had better survival rates than those in third class.

This shows that both gender and ticket class may have influenced someone's chances of surviving the disaster.

### Data Humanism

Although the chart uses percentages, the data represents real people who experienced the Titanic disaster.

I think the visualisation could be more personal by including some passenger names, ages or individual stories alongside the chart. This would help remind the viewer that each part of the graph represents actual people rather than just numbers.

### Reflection

The chart is useful for comparing survival between different groups, but it doesn't explain exactly why these differences happened. Other factors such as age or access to lifeboats could also have played a role.
