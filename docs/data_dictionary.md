# Data Dictionary

Source: [Melbourne Housing Snapshot (Kaggle)](https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot)
File: `data/melb_data.csv`
Column names are kept as in the original file, including the original
spelling (`Lattitude`, `Longtitude`).


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
