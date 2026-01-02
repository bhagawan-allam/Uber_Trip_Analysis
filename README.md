# Uber Trip Analysis Dashboard (Power BI & Excel)

### Dashboard Link : NA(Please Refer PDF)

## Project Overview
This Power BI dashboard provides a comprehensive analysis of Uber trip data for June 2024, delivering key insights into booking patterns, revenue generation, trip distance/time efficiency, and customer preferences across New York City.

The solution enables data-driven decisions to improve operations, resource planning, and customer service.
### Steps followed 

**Data Collection & Import**
- Uber Trip Details.xlsx: Core trip data (date, time, distance, value, vehicle, payment, passengers, pickup location).

- Location Table.xlsx: Supplemental mapping of key pickup/drop-off locations.

- Problem Statement.docx: Requirements, KPIs, and visual goals.

**Data Cleaning & Preparation (SQL)**
- Removed duplicate and null entries.

- Ensured consistency in vehicle types and payment method values.

- Extracted time features (hour, day, weekday) from pickup time.

- Joined location table for geo-level analysis.
    
**Data Modeling & Transformation (Power BI)**
- Created relationships between vehicle, time, and location dimensions.

- Used disconnected tables for dynamic KPI switching via slicers.
  
**Dashboard Development (Power BI & DAX)**

✅ KPI Cards (Total Bookings, Revenue, Distance, Time)

✅ Vehicle Type Grid Comparison

✅ Day/Night Trip Analysis

✅ Payment Type Distribution

✅ Pickup/Drop-off distribution

✅ Peak Hours & Daily Trends (Area Charts)

✅ Most Preferred Vehicle per Location

✅ Most Frequent Pickup/Drop-off Points

✅ Drill-Through Details Tab with Trip-Level Data

✅ Tooltips, Dynamic Titles, Slicers, Bookmarks, and Clear Filters

## Few Complex DAX formulas used for KPIs creation(Refer Project Doc for more DAX)

  
**Average Trip Distance** = 

VAR AvgMiles = ROUND(AVERAGE('Trip Details'[trip_distance]),0)

RETURN 
CONCATENATE(AvgMiles, " miles")

Average Trip Time = VAR AvgTime = AVERAGEX(
                                'Trip Details', DATEDIFF('Trip Details'[Pickup Time], 'Trip Details'[Drop Off Time], MINUTE)
)

RETURN
CONCATENATE(FORMAT(AvgTime, "0"), "min")
 
  -----------------------------------------------------------------------------------------
**Farthest Trip** = 
VAR MaxDistance = MAX('Trip Details'[trip_distance])

VAR PickupLocation = 
    LOOKUPVALUE(
        'Location Table'[Location],
        'Location Table'[LocationID],
        CALCULATE(
            SELECTEDVALUE('Trip Details'[PULocationID]),
            'Trip Details'[trip_distance] = MaxDistance
        )
    )

VAR DropoffLocation = 
    LOOKUPVALUE(
        'Location Table'[Location],
        'Location Table'[LocationID],
        CALCULATE(
            SELECTEDVALUE('Trip Details'[DOLocationID]),
            'Trip Details'[trip_distance] = MaxDistance
        )
    )

RETURN
    "Picup: " & PickupLocation & " → Drop-off: " & DropoffLocation & " (" & FORMAT(MaxDistance, "0.0") & "miles)"

----------------------------------------------------------

**Most Frequent Dropoff Point** = 
VAR DropOffCounts = 
    ADDCOLUMNS(
        SUMMARIZE(
            'Trip Details',
            'Location Table'[Location]
        ),
        "DropOffCounts",
        CALCULATE(
            COUNT('Trip Details'[Trip ID]),
            USERELATIONSHIP('Trip Details'[DOLocationID],'Location Table'[LocationID])
            )
    )
    
VAR RankedDropOffs = 
     ADDCOLUMNS(
        DropOffCounts,
        "Rank",
        RANKX(DropOffCounts, [DropOffCounts],,DESC,Dense)
     )

VAR TopDropoff = 
    FILTER(RankedDropOffs, [Rank] = 1)

RETURN 
    CONCATENATEX(TopDropoff, 'Location Table'[Location], ", ")

-----------------------------------------------------

Most Frequent Pickup Point = 

VAR Pickpoint = TOPN(1,
                         SUMMARIZE(
                            'Trip Details', 'Location Table'[Location], "Pickup Point", COUNT('Trip Details'[Trip ID])
                         ),
                         [Pickup Point], DESC
)
RETURN CONCATENATEX(Pickpoint, 'Location Table'[Location], ",")

------------------------------------------------------------------
 
 # Report Snapshot (Power BI DESKTOP)

 
![image](https://github.com/user-attachments/assets/749b843b-5392-4328-9a88-154c9fd03ab6)


![image](https://github.com/user-attachments/assets/963384d7-3c1c-4a29-89cb-2d33fa1a02bb)


![image](https://github.com/user-attachments/assets/d4667a29-512a-4be1-9361-c4a1dc79325a)

## Key Insights & Findings
**📍 Location & Vehicle Insights**
- Top Pickup Location: Penn Station / Madison Sq West

- Top Drop-off Location: Upper East Side North

- Farthest Trip: 144.1 miles (Lower East Side → Crown Heights North)

- Most Preferred Vehicle: UberX (38,744 bookings)

**💳 Payment Breakdown**
- Uber Pay: 67.03% of bookings

- Cash: 32.23%

- Amazon Pay & Google Pay: <1%

**🌇 Trip Types**
- Day Trips: 65.28%

- Night Trips: 34.72%

**🕒 Time Trends**
- Highest bookings: Weekends, 11 AM – 3 PM

- Consistent peak on Saturday and Sunday mornings and afternoons

- Steady rise from 6 AM, peaking at mid-day
## Technology Stack
- Power BI Desktop – Report building, DAX modeling

- Power Query – Data transformation (ETL)

- Excel – Data source formatting

- DAX – KPIs, calculated columns, dynamic measures
## Dashboard Features
✅ Booking & Revenue KPIs (Total Bookings, Total Value, Avg Distance/Time)

✅ Trip Type Analysis (Day vs. Night trips comparison)

✅ Payment Method Breakdown (Uber Pay, Cash, Amazon/Google Pay)

✅ Vehicle Type Performance (Bookings, Revenue, Distance by type)

✅ Location Insights (Top Pickup/Drop-off Points, Borough Distribution)

✅ Time-Based Trends (Hourly & Daily patterns via heatmaps and area charts)

✅ Farthest Trip Detection (Outlier trip distance analysis)

✅ Dynamic Measure Selector (Switch visuals between Bookings, Revenue, Distance)

✅ Interactive Filters & Slicers (Date, City, Vehicle Type, Payment Method)

✅ Drill-Through Detail Tab (Trip-level data exploration
