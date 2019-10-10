
### The Stanford Data Project Analysis - Nashville, TN
On a typical day in the United States, police officers make more than 50,000 traffic stops. Our team is gathering, analyzing, and releasing records from millions of traffic stops by law enforcement agencies across the country. Our goal is to help researchers, journalists, and policymakers investigate and improve interactions between police and the public.

### [1. Purpose of the analysis](#1.Purpose-of-Analysis)
### [2. Data Acquisition](#2.Data-Acquisition )
### [3. Exploratory Data Analysis](#3.Exploratory-Data-Analysis)
### [4. Analysis Questions](#4.Analysis-Questions)


### 1.Purpose-of-Analysis
The purpose of the analysis is kind of hollistic approach to explore what we can get out of the data, so I am not sure if there is a specific purpose or question. I will insert all the questions that come to my mind below and will update this notebook frequently


```python
import pandas as pd
import requests, zipfile, io
```

### Data aquisition
The data of the project is in a form of compressed file hosted online, it will be downloaded and extracted to the project directory. You can always host the data anywhere else and change the pointer in the read csv  line


```python
url="https://stacks.stanford.edu/file/druid:hp256wp2687/hp256wp2687_tn_nashville_2019_08_13.csv.zip"
```


```python
r = requests.get(url)
z = zipfile.ZipFile(io.BytesIO(r.content))
z.extractall()
```


```python
tn_raw=pd.read_csv(z.namelist()[0], low_memory=False)
```

### 1-Data_Cleaning
In this step we will select the column of interest and  will drop all the na values in these columns.


```python
tn=tn_raw.copy()
```


```python
tn.shape
```




    (3092351, 42)




```python
tn.isnull().sum()
```




    raw_row_number                          0
    date                                    0
    time                                 5467
    location                                0
    lat                                187106
    lng                                187106
    precinct                           390222
    reporting_area                     332393
    zone                               390222
    subject_age                           839
    subject_race                         1850
    subject_sex                         12822
    officer_id_hash                        11
    type                                    0
    violation                            8020
    arrest_made                            28
    citation_issued                       320
    warning_issued                        337
    outcome                              1935
    contraband_found                  2964646
    contraband_drugs                  2964646
    contraband_weapons                2964646
    frisk_performed                        22
    search_conducted                       39
    search_person                          43
    search_vehicle                         41
    search_basis                      2964646
    reason_for_stop                      8020
    vehicle_registration_state          31791
    notes                             2579696
    raw_verbal_warning_issued             337
    raw_written_warning_issued         493091
    raw_traffic_citation_issued           321
    raw_misd_state_citation_issued     693814
    raw_suspect_ethnicity                  77
    raw_driver_searched                    30
    raw_passenger_searched                  9
    raw_search_consent                     21
    raw_search_arrest                      13
    raw_search_warrant                      0
    raw_search_inventory                    2
    raw_search_plain_view                   7
    dtype: int64




```python

```


    ---------------------------------------------------------------------------

    MemoryError                               Traceback (most recent call last)

    <ipython-input-109-3bff5300f884> in <module>
    ----> 1 tn-tn['search_person']
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\ops.py in f(self, other, axis, level, fill_value)
       2028             return _combine_series_frame(self, other, pass_op,
       2029                                          fill_value=fill_value, axis=axis,
    -> 2030                                          level=level)
       2031         else:
       2032             if fill_value is not None:
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\ops.py in _combine_series_frame(self, other, func, fill_value, axis, level)
       1928 
       1929         # default axis is columns
    -> 1930         return self._combine_match_columns(other, func, level=level)
       1931 
       1932 
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\frame.py in _combine_match_columns(self, other, func, level)
       5112         assert isinstance(other, Series)
       5113         left, right = self.align(other, join='outer', axis=1, level=level,
    -> 5114                                  copy=False)
       5115         assert left.columns.equals(right.index)
       5116         return ops.dispatch_to_series(left, right, func, axis="columns")
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\frame.py in align(self, other, join, axis, level, copy, fill_value, method, limit, fill_axis, broadcast_axis)
       3790                                             method=method, limit=limit,
       3791                                             fill_axis=fill_axis,
    -> 3792                                             broadcast_axis=broadcast_axis)
       3793 
       3794     @Substitution(**_shared_doc_kwargs)
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\generic.py in align(self, other, join, axis, level, copy, fill_value, method, limit, fill_axis, broadcast_axis)
       8426                                       copy=copy, fill_value=fill_value,
       8427                                       method=method, limit=limit,
    -> 8428                                       fill_axis=fill_axis)
       8429         else:  # pragma: no cover
       8430             raise TypeError('unsupported type: %s' % type(other))
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\generic.py in _align_series(self, other, join, axis, level, copy, fill_value, method, limit, fill_axis)
       8523 
       8524                 if lidx is not None:
    -> 8525                     fdata = fdata.reindex_indexer(join_index, lidx, axis=0)
       8526             else:
       8527                 raise ValueError('Must specify axis=0 or 1')
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\internals\managers.py in reindex_indexer(self, new_axis, indexer, axis, fill_value, allow_dups, copy)
       1229         if axis == 0:
       1230             new_blocks = self._slice_take_blocks_ax0(indexer,
    -> 1231                                                      fill_tuple=(fill_value,))
       1232         else:
       1233             new_blocks = [blk.take_nd(indexer, axis=axis, fill_tuple=(
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\internals\managers.py in _slice_take_blocks_ax0(self, slice_or_indexer, fill_tuple)
       1293 
       1294                 blocks.append(self._make_na_block(placement=mgr_locs,
    -> 1295                                                   fill_value=fill_value))
       1296             else:
       1297                 blk = self.blocks[blkno]
    

    ~\AppData\Local\Continuum\anaconda3\lib\site-packages\pandas\core\internals\managers.py in _make_na_block(self, placement, fill_value)
       1323 
       1324         dtype, fill_value = infer_dtype_from_scalar(fill_value)
    -> 1325         block_values = np.empty(block_shape, dtype=dtype)
       1326         block_values.fill(fill_value)
       1327         return make_block(block_values, placement=placement)
    

    MemoryError: 


## Example2

### Data Exploration
In this section we will perform some basic data analysis and exploration tasks in order to understand the data. worth noting also that data comes with shape file that will help in data spatial distrubution.
#### Examining and fixing the data types


```python
tn.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>raw_row_number</th>
      <th>date</th>
      <th>time</th>
      <th>location</th>
      <th>lat</th>
      <th>lng</th>
      <th>precinct</th>
      <th>reporting_area</th>
      <th>zone</th>
      <th>subject_age</th>
      <th>...</th>
      <th>raw_traffic_citation_issued</th>
      <th>raw_misd_state_citation_issued</th>
      <th>raw_suspect_ethnicity</th>
      <th>raw_driver_searched</th>
      <th>raw_passenger_searched</th>
      <th>raw_search_consent</th>
      <th>raw_search_arrest</th>
      <th>raw_search_warrant</th>
      <th>raw_search_inventory</th>
      <th>raw_search_plain_view</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>232947</td>
      <td>2010-10-10</td>
      <td>NaN</td>
      <td>DOMINICAN DR &amp; ROSA L PARKS BLVD, NASHVILLE, T...</td>
      <td>36.187925</td>
      <td>-86.798519</td>
      <td>6</td>
      <td>4403.0</td>
      <td>611</td>
      <td>27.0</td>
      <td>...</td>
      <td>False</td>
      <td>NaN</td>
      <td>N</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>237161</td>
      <td>2010-10-10</td>
      <td>10:00:00</td>
      <td>1122 LEBANON PIKE, NASHVILLE, TN, 37210</td>
      <td>36.155521</td>
      <td>-86.735902</td>
      <td>5</td>
      <td>9035.0</td>
      <td>513</td>
      <td>18.0</td>
      <td>...</td>
      <td>True</td>
      <td>NaN</td>
      <td>N</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>232902</td>
      <td>2010-10-10</td>
      <td>10:00:00</td>
      <td>898 DAVIDSON DR, , TN, 37205</td>
      <td>36.117420</td>
      <td>-86.895593</td>
      <td>1</td>
      <td>5005.0</td>
      <td>121</td>
      <td>52.0</td>
      <td>...</td>
      <td>False</td>
      <td>NaN</td>
      <td>N</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>233219</td>
      <td>2010-10-10</td>
      <td>22:00:00</td>
      <td>MURFREESBORO PIKE &amp; NASHBORO BLVD, ANTIOCH, TN...</td>
      <td>36.086799</td>
      <td>-86.648581</td>
      <td>3</td>
      <td>8891.0</td>
      <td>325</td>
      <td>25.0</td>
      <td>...</td>
      <td>False</td>
      <td>NaN</td>
      <td>N</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>232780</td>
      <td>2010-10-10</td>
      <td>01:00:00</td>
      <td>BUCHANAN ST, NORTH, TN, 37208</td>
      <td>36.180038</td>
      <td>-86.809109</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.0</td>
      <td>...</td>
      <td>False</td>
      <td>NaN</td>
      <td>N</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 42 columns</p>
</div>




```python
tn.subject_race.value_counts(normalize=True)
```




    white                     0.540648
    black                     0.377243
    hispanic                  0.053329
    asian/pacific islander    0.013483
    unknown                   0.011933
    other                     0.003364
    Name: subject_race, dtype: float64




```python
tx['date']=pd.to_datetime(tx['date'])
tx.dtypes
```




    date               datetime64[ns]
    lat                       float64
    lng                       float64
    subject_race               object
    subject_sex                object
    type                       object
    citation_issued              bool
    speed                     float64
    vehicle_color              object
    vehicle_make               object
    dtype: object




```python
tx.set_index('date',inplace=True)
```


```python
tx.subject_race.value_counts(normalize=1)*100
```




    white                     62.663390
    black                     28.913622
    unknown                    4.787207
    asian/pacific islander     3.456157
    other                      0.179624
    Name: subject_race, dtype: float64




```python
tx.subject_sex.value_counts(normalize=1)*100
```




    male      61.754243
    female    38.245757
    Name: subject_sex, dtype: float64


