# Aircraft Flight Schedules - ADS-B Derived

Datasets featuring global, **_high-level_** flight schedules extracted from worldwide aircraft ADS-B position transmissions.

Published per quarter of a year, starting from 2024+ onwards. Covers all flights globally as long as within coverage of the [ADSBlol](https://github.com/adsblol) initiative.

![from adsb to flightschedules_eham example final](https://github.com/user-attachments/assets/6ec77121-e02c-43f4-99d0-134e8e9db8e2)


# Data Sources
1) This project uses the ADS-B data from the [ADSBlol](https://github.com/adsblol) initiative. Consider supporting their great project.
2) This project uses validation data from [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01) to check extracted routes with additional route data (based on aircraft callsign). Again, consider supporting this initiative.


# Data Processing
Each day, ADSBlol publishes ADS-B data in two versions: prod-0 and staging-0. The largest file (by file size) is selected for each day.

After extracting the data, ADS-B transmissions are retained for all aircraft, but only about 1 in every 4 messages per aircraft is kept - more specifically the detailed ones, not the basic intermediate transmissions. This ensures that processing the cumulative data for each quarter of a year remains feasible:
![full transmission only](https://github.com/user-attachments/assets/b029b3a1-c431-4c2a-8b52-21170b2b1d30)


# Where to Get the Extracted Flight Datasets?
See the [Releases](https://github.com/MrAirspace/aircraft-flight-logs/releases) section of this repository for a parquet file with the flights per aircraft, per quarter of a year.


# How to Load the Extracted Flight Dataset?
The parquet filetype has been selected to keep flights data manageable in terms of size and processing/loading times. Each quarter features approx. 10-13+ million flights and ~500,000 aircraft, which in csv format would total approx. 3 GB. Hence the selection of a parquet filetype, which stays far below 1 GB.
Loading a parquet file is very straightforward with python:

`df = pandas.read_parquet('2024_Q1.parquet')`

Furthermore, to check the parquet dataset without python, you can use tools like [ParquetViewer](https://github.com/mukunku/ParquetViewer) which feature a user interface/GUI and can be installed on Windows as exe.


# Data Timeframes
The data is published per quarter of a year. The 4 quarters of each year feature some overlap to ensure flights are complete (not cut in half at the quarter boundaries). Thereby, when combining the quarterly files to a yearly schedule, there will be some overlap in flights.

Do not use 'df.drop_duplicates()', but instead identify overlap flights by checking whether the entry in the df column 'Track_Origin_DateTime_UTC' falls within the quarter months of the pertaining parquet file:

```
df['Month_UTC'] = pandas.to_datetime(df['Track_Origin_DateTime_UTC'], errors = 'coerce').dt.month

if str(file).endswith('Q1.parquet'):
    df['Inside_Season'] = df['Month_UTC'].isin([1, 2, 3])
elif ... (for the other quarters)
```


# Data Coverage
_Status Q2 2024_
![image](https://github.com/user-attachments/assets/92117619-ecc2-48f3-bc73-07407cca4445)
Number of receivers/antennas of ADSBlol initiative (image above)

![image](https://github.com/user-attachments/assets/b96a126c-00aa-4076-9882-f5a84669eb13)
Aircraft coverage of ADSBlol initiative. Time of day ~13:00 UTC to have reasonable ops in all continents - no midnight situation in major markets (image above)


# Data Enrichment - Against Limited Coverage in Certain Areas
Given potentially limited ADS-B reception coverage of the ADSBlol initiative in certain continents, some aircraft tracks start after the airport of origin or end before the airport of destination. For those cases, the flights data has been enhanced by looking up the aircraft flight callsign and matching it with the open-source aircraft callsign vs route dataset of [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01).


# Data Enrichment - GPS Spoofing
Given ADS-B transmissions simply sending location data, wrong location data as a result of GPS spoofing can also be transmitted. Once more, the added column with callsign vs route lookup allows to filter out those flights where aircraft emitted wrong position data.


# License
Please use in line with the license defined in this repository. No guarantee, no liability, no warranty. All open-source.


# Contact
For questions or citation, please refer to my LinkedIn profile [Sebastiaan Menger - LinkedIn](https://de.linkedin.com/in/sebastiaanmenger)


# Details - What are the Date/Times in the Datasets?
This concerns high-level/approximated RWY times in UTC, so lift-off time for departures and touchdown time for arrivals. This is generally reference to the first 'ground' entry for arrivals, and the last 'ground' entry for departures. However, there can also be cases with more limited ADS-B coverage, where the track does not start or stop at the airport:

![image](https://github.com/user-attachments/assets/6c5e04a3-3268-4d6f-91cc-9f1ab479028b)

For those cases, the beginning/end of the track has been selected as the time of the flight. For further implications, see section below.


# Details - Why are There Multiple Airports Listed for a Flight?
Similar to the section above, for those cases where the track does not start or stop at the airport, multiple airports in the vicinity of the first/last position of the ADS-B track have been listed as options.
To nevertheless determine the plausible airport of origin/destination, validation data from [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01) has been included to match the aircraft flight callsign with external route data.


# Details - How are Go-Arounds Considered?
In case of go-arounds/touch-and-go/balked landings, only the final touchdown is counted as touchdown time of the flight (with commercial flights in mind).

![image](https://github.com/user-attachments/assets/96de9c02-a204-4d1e-8198-3cb0069e93e2)
