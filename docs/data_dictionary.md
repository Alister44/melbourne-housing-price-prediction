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
