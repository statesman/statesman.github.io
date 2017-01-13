---
layout: post
title: "School ratings in pandas"
date: 2017-01-13 10:00:00 -0500
categories: data-analysis
tags: notebooks
byline:
    - name: "Dan Hill"
      twitter: "danhillreports"
---

How do school ratings differ between local charter and traditional public schools?

Finding an answer involved learning new a pandas trick and writing a python geospatial query.

Reporters Melissa Taboada and Julie Chang investigated charter school trends after the Texas Education Agency released preliminary results of its new school rating system. We were also interested in comparing the preliminary A-F grades to the [previous system](http://www.statesman.com/news/local/four-austin-middle-schools-stumble-texas-education-ratings/N53OLKMxv8v6xvAe2kNARM/).

The previous system rated schools based on whether they met target scores in four categories. The proposed A-F system assigns schools scores in five categories and converts those numerical scores to letter grades. Although the systems are considerably different, we decided compare:

- How many schools missed at least one target score under the old system?
- How many received a D or F in any category of the new system?

We would also examine differences between those counts when aggregated by charter and “alternative education” school status.

I used pandas to assign and count schools by catecory. Both ratings systems published data as spreadsheets with a row for each school and a column for each category grade. I assigned binary values to represent if a school received a D or F or missed a target by concatenating the grade columns and testing the resulting string:

``` python
# Concatenate column values
dat['all_grades'] = (dat['D1 Grade'] +
                     dat['D2 Grade'] +
                     dat['D3 Grade'] +
                     dat['D4 Grade'])

# Assign new variable
dat['d_or_f'] = (dat['all_grades'].str
                                  .contains('D|F')
                                  .astype(str)
                                  .replace({
                                    'True': 'D or F',
                                    'False': 'No Ds or Fs'
                                    })
                )

dat[['all_grades', 'd_or_f']].head()
```

<p>
<div>
<table border="1" class="data">
  <thead>
    <tr style="text-align: right;">
      <th>all_grades</th>
      <th>d_or_f</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>BDDC</td>
      <td>D or F</td>
    </tr>
    <tr>
      <td>CDDC</td>
      <td>D or F</td>
    </tr>
    <tr>
      <td>DDCF</td>
      <td>D or F</td>
    </tr>
    <tr>
      <td>BCCD</td>
      <td>D or F</td>
    </tr>
    <tr>
      <td>BBCC</td>
      <td>No Ds or Fs</td>
    </tr>
  </tbody>
</table>
</div>
</p>


Then I could count schools broken down by that binary variable as well as charter and AEA status. Even though it’s example in the official pandas crosstab documentation, I had never before realized I could crosstab more than two variables by passing lists:


```python
pd.crosstab(
    dat.fail_grade,
    [dat.is_charter, dat.is_aea]
)
```

<p>
<div>
<table border="1" class="data">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">Charter</th>
      <th colspan="2" halign="left">Non-Charter</th>
    </tr>
    <tr>
      <th></th>
      <th class="num">AEA</th>
      <th class="num">Non-AEA</th>
      <th class="num">AEA</th>
      <th class="num">Non-AEA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>D or F</th>
      <td class="num">59</td>
      <td class="num">293</td>
      <td class="num">81</td>
      <td class="num">4535</td>
    </tr>
    <tr>
      <th>No Ds or Fs</th>
      <td class="num">90</td>
      <td class="num">247</td>
      <td class="num">153</td>
      <td class="num">3167</td>
    </tr>
  </tbody>
</table>
</div>
</p>

We wanted to apply this method only to schools in the Austin area. Because charter schools can be members of school districts that are based in other Texas regions, we decided a geospatial query would be the most reliable way to identify schools in the Austin area. I used `fiona` to read shapefiles of Texas schools and education regions, `pyproj` to reproject schools points to the same coordinate system as the regions and shapely to test if schools are located inside the Austin education region:

```python
import fiona
import pyproj

from shapely.geometry import Point, shape
from shapely.prepared import prep

shp = fiona.open('PATH/TO/ESCRegions.shp')
shp_schools = fiona.open('PATH/TO/Current_Schools.shp')

# Get the Austin region shape as a shapely object
shp_atx = prep(
    shape(
        list(
            filter(
                lambda x: x['properties']['REGION'] == '13',
                shp
                )
            )[0]['geometry']
        )
    )

region_school_ids = []

# Get the desired projection
proj_shp = pyproj.Proj(shp.crs)

for school in shp_schools:
    # Reproject the school latitude/longitude
    transformed = proj_shp(
        school['geometry']['coordinates'][0],
        school['geometry']['coordinates'][1]
    )

    # Convert the reprojected point to a shapely point
    school_point = Point(
        transformed
    )

    # Test if the shapely point is in the region shape
    if shp_atx.contains(school_point):
        region_school_ids.append(school['properties']['CAMPUS'])
```

I then used the list of IDs to limit our analysis to Austin-area schools that received grades in both systems.


