# Overview

## Bar Chart

```python
trend_bar = alt.Chart(trends).mark_bar().encode( 
  x="search_term", 
  y="value", 
  color="search_term" 
) 
```

## Line Chart

```python
trend_line = alt.Chart(trends).mark_line().encode( 
  x="yearmonth(date):T", 
  y="mean(value)", 
  color="search_term" 
) 
```

## Scatter Plot Chart

```python
scatter_countries = alt.Chart(lifecountries).mark_circle().encode(
    x="Country GDP",
    y="Life Expectancy",
    color="Continent",
    size="size"
)
```

## Map Chart

```python
temp = world_temp.groupby("city").mean().reset_index() 
points = alt.Chart(temp).mark_circle().encode( 
    latitude="lat", 
    longitude="lng",
    color=alt.Color("mean(temp)", scale=alt.Scale(domain=[-30,10,40],range=["lightblue","orange","red"]))
) 
# remote geojson data object 
url = "https://raw.githubusercontent.com/datasets/geo-countries/master/data/countries.geojson" 
data_geojson_remote = alt.Data(url=url, format=alt.DataFormat(property='features',type='json')) 

# chart object 
background = alt.Chart(data_geojson_remote).mark_geoshape( 
  fill="lightgrey", 
  stroke="white" 
).encode( 
  color="properties.name:N" 
).properties( 
  projection={'type': 'identity', 'reflectY': True} 
) 

background+points
```

## Histogram Chart

```python
hist_brain = alt.Chart(brain).mark_bar().encode(
    x=alt.X("Brain Weight", bin=alt.Bin(maxbins=100)),
    y="count()"
)
```

## Composition Horizontal

```python
trend_bar|trend_line
```

## Composition Vertical

```python
trend_bar&trend_line
```

## Composition Layer

```python
background+points
```

## Selection Single

```python
select_term = alt.selection(type="single",encodings=["x"])

trend_line = alt.Chart(trends).mark_line().encode(
    x="yearmonth(date):T",
    y="mean(value)",
    color="search_term"
).transform_filter(
    select_term
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term",
    tooltip="search_term"
).properties(
    selection=select_term
)

trend_bar|trend_line
```

## Selection interval

```python
select_date = alt.selection(type="interval",encodings=["x"])

trend_line = alt.Chart(trends).mark_line().encode(
    x="yearmonth(date):T",
    y="mean(value)",
    color="search_term"
).properties(
    selection=select_date
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term",
    tooltip="search_term"
).transform_filter(
    select_date
)

trend_bar|trend_line
```

