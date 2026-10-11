# Data Dictionary

Source: [Melbourne Housing Snapshot (Kaggle)](https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot)
File: `data/melb_data.csv`
Column names are kept as in the original file, including the original
spelling (`Lattitude`, `Longtitude`).

| Column | Description | Data type | Values / unit | Project notes |
|---|---|---|---|---|
| `Suburb` | Suburb of the property | text | – | High cardinality, frequency encoding |
| `Address` | Street address of the property | text | – | Not used in the model |
| `Rooms` | Number of rooms | numeric | count | |
| `Type` | Property type | text | h = house, cottage, villa, semi, terrace; u = unit, duplex; t = townhouse | The source also lists br, dev site and o res, but only h, u and t appear in the data |
| `Price` | Sale price in dollars | numeric | AUD (assumed) | **Target variable** |
| `Method` | Sale method | text | S = sold; SP = sold prior; PI = passed in; VB = vendor bid; SA = sold after auction | The source defines 10 codes, only these 5 appear in the data. PI and VB excluded |
| `SellerG` | Real estate agent | text | – | Not used in the model |
| `Date` | Date sold | date | day/month/year | Parsed with `dayfirst=True` |
| `Distance` | Distance from the city center (CBD, Central Business District) | numeric | km (assumed) | |
| `Postcode` | Postal code of the property | numeric | – | A code, not a quantity |
| `Bedroom2` | Number of bedrooms, scraped from a different source | numeric | count | Almost duplicates `Rooms`, dropped |
| `Bathroom` | Number of bathrooms | numeric | count | |
| `Car` | Number of car spots | numeric | count | 0.46% missing, imputed in the pipeline |
| `Landsize` | Land size (plot area) | numeric | m² (assumed) | Zeros kept: a property can come without its own land |
| `BuildingArea` | Building size | numeric | m² (assumed) | Zeros and implausible small values set to missing |
| `YearBuilt` | Year the property was built | numeric | year | Impossible years set to missing |
| `CouncilArea` | Governing council area of the property | text | – | |
| `Lattitude` | Latitude of the property | numeric | degrees | |
| `Longtitude` | Longitude of the property | numeric | degrees | |
| `Regionname` | General region of Melbourne | text | West, North West, North, North East, etc. | |
| `Propertycount` | Number of properties in the suburb | numeric | count | |

## Method codes
| Code | Meaning |
|---|---|
| S | Property sold |
| SP | Property sold prior |
| PI | Property passed in |
| VB | Vendor bid |
| SA | Sold after auction |

Other codes defined by the source (PN, SN, NB, W, SS) do not appear in this file.

| Column | Description | Unit / values |
|---|---|---|
| `Suburb` | Suburb where the property is located | text |
| `Address` | Street name and house number | text |
| `Rooms` | Number of rooms | count |
| `Type` | Property type. Only h, t and u appear in the data | h = house, t = townhouse, u = unit/duplex |
| `Price` | Sale price in dollars (currency is not specified in the source) | dollars |
| `Method` | Sale method. Only S, SP, PI, VB and SA appear in the data | see [Method codes](#method-codes) |
| `SellerG` | Surname of the real estate agent | text |
| `Date` | Date of the sale or, most likely, of the auction | day/month/year |
| `Distance` | Distance from the city center (Central Business District, CBD) | km (assumed) |
| `Postcode` | Postal code of the property | code |
| `Bedroom2` | Number of bedrooms, scraped from a different source | count |
| `Bathroom` | Number of bathrooms | count |
| `Car` | Number of car spots | count |
| `Landsize` | Land size (plot area) of the property | m² (assumed) |
| `BuildingArea` | Building size | m² (assumed) |
| `YearBuilt` | Year of construction | year |
| `CouncilArea` | Governing council area of the property | text |
| `Lattitude` | Latitude of the property | degrees |
| `Longtitude` | Longitude of the property | degrees |
| `Regionname` | General region of Melbourne (West, North West, North, North East, etc.) | text |
| `Propertycount` | Number of properties in the suburb | count |

## Method codes
| Code | Meaning |
|---|---|
| S | Property sold |
| SP | Property sold prior |
| PI | Property passed in |
| VB | Vendor bid |
| SA | Sold after auction |

The source defines five more codes (PN, SN, NB, W, SS), but they do not
appear in this file.

## Column notes

### `Rooms` and `Bedroom2`
In 1.5% of rows `Bedroom2` is greater than `Rooms`, which looks illogical.
`Bedroom2` comes from a different source, so the difference may reflect
different definitions rather than errors. Since the two columns are almost
identical, I kept `Rooms` and dropped `Bedroom2`. I did not remove any rows.

### `Bathroom` and `Rooms`
In 28 rows (0.2%) `Bathroom` is greater than `Rooms`. In 23 of them the
difference is 1, which is unusual but possible. In 5 rows the difference is
2 or more. I did not change these rows, because the effect on the model is
negligible and I cannot tell which of the two columns is wrong.

### `Price` and `Method`
Only S, SP and SA clearly mean that the sale took place.
- **PI (passed in):** the property did not sell at auction. However, PI rows
  have a value in `Date`, which looks contradictory.
- **VB (vendor bid):** the outcome is not clearly stated, so the property
  may not have been sold.

For PI and VB, `Price` may reflect the last or highest bid instead of the
final sale price. The goal of the model is to predict the sale price, so
these rows would add noise to the target. I excluded PI and VB rows
(X rows, Y% of the data).

### `Date`
The source describes `Date` as the date sold. After examining `Method`, I
concluded that it is not necessarily the sale date: PI and VB rows also have
a date, which suggests that `Date` may be the date of the auction, where the
sale did not always take place. I could not confirm this from the data.

### `Distance`
Distance from the property to the city center. The source does not state the
unit; I assume kilometers. Distance is expected to be one of the main price
drivers: prices are usually higher closer to the center, although the
relationship is not always direct and depends on the area as well.

### `Car`
`Car` has 0.46% missing values. I left them unchanged in the cleaning step;
they are imputed with the median inside the pipeline, fitted on the
training data only.

### `Landsize`
I treat `Landsize` as the plot area of the property, in square meters.
I kept zeros. In Australia it is common for houses, townhouses and duplexes
to come without a private plot of land, so 0 is treated as a real value, not
as missing data. Zeros make up 1.8% of houses, 12.5% of townhouses and 54%
of units.

### `BuildingArea`
An area of 0 is not a real measurement, so I replaced X zeros with missing
values. I also replaced Y values below 15 m² for properties with more than
one room, which is not realistic. Properties with one room and an area of
10-15 m² were kept, because a studio of that size is unusual but possible.
All missing values are imputed with the median in the pipeline.

### `YearBuilt`
I checked whether `YearBuilt` is specified correctly by displaying a few
rows in ascending order. The year 1196 is clearly an error. The next value,
1830, looks plausible, so I set years below <threshold> to missing.
