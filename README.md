# Aircraft Flight Schedules
Datasets featuring global, **_high-level_** flight schedules extracted from worldwide aircraft ADS-B position transmissions.

Published per quarter of a year, starting from 2024+ onwards. Covers all flights globally as long as within coverage of the [ADSBlol](https://github.com/adsblol) initiative.

![from adsb to flightschedules_eham example final](https://github.com/user-attachments/assets/6ec77121-e02c-43f4-99d0-134e8e9db8e2)


# Data Sources
1) This project uses the ADS-B data from the [ADSBlol](https://github.com/adsblol) initiative. Consider supporting their great project.
2) This project uses validation data from [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01) to check extracted routes with additional route data (based on aircraft callsign). Again, consider supporting this initiative.


# Data Processing
Each day, ADSBlol publishes ADS-B data in two versions: prod-0 and staging-0. The largest file (by file size) is selected for each day.

After extracting the data, ADS-B transmissions are retained for all aircraft, but only about 1 in every 4 messages per aircraft is kept - more specifically the detailed ones, not the basic intermediate transmissions. This primarily affects the accuracy of the enroute phase which for extraction of arrival and departure data of a flight is of less relevance anyway. At the same time, this ensures that processing the cumulative data for each quarter of a year remains feasible:
![full transmission only](https://github.com/user-attachments/assets/b029b3a1-c431-4c2a-8b52-21170b2b1d30)


# Where to Get the Extracted Flight Datasets?
See the [Releases](https://github.com/MrAirspace/aircraft-flight-logs/releases) section of this repository for a parquet file with the flights per aircraft, per quarter of a year.


# How to Load the Extracted Flight Dataset?
The parquet filetype has been selected to keep flights data manageable in terms of size and processing/loading times. Each quarter features approx. 10-15+ million flights and ~500,000 aircraft, which in csv format would total approx. 3+ GB. Hence the selection of a parquet filetype, which stays below 1 GB.
Loading a parquet file is very straightforward with python:

`df = pandas.read_parquet('2024_Q1.parquet')`

Furthermore, to check the parquet dataset without python, you can use tools like [ParquetViewer](https://github.com/mukunku/ParquetViewer) which feature a user interface/GUI and can be installed on Windows as exe.


# The Flight Schedules - Which Column is What?
Below an explanation of which column features which data:
![columns_arr](https://github.com/user-attachments/assets/39cadb32-6b70-44f8-9e1f-ac1429b652aa)
'ICAO_Hex' = the unique 24-bit hexadecimal address assigned to the aircraft transponder

'Reg' = the registration (tail number) of the aircraft

'AC_Type' = the ICAO aircraft type code

AC_Type_Description' = the full aircraft type designator inlcuding manufacturer

'Airline' = the airline ICAO code, as derived from the callsign

'Callsign' = the callsign of the flight, basically speaking a flight number for ATC purposes

'Track_Origin_Lat' = starting point of the track of a flight (further definition in sections below) - latitude in decimal format

'Track_Origin_Lon' = starting point of the track of a flight (further definition in sections below) - longitude in decimal format

'Track_Origin_FL_Ft' = starting flight level of the track of a flight - can be 'ground' or flight level (depending on the ADS-B antenna coverage - see possible cases in sections below)

'Track_Origin_DateTime_UTC' = starting date and time of the track of a flight - UTC format

'Track_Origin_ApplicableAirports' = airport(s) in the vicinity of the starting point of the track of a flight - airport ICAO code (further details and possible cases in sections below)


![columns_dep_and_route_validation](https://github.com/user-attachments/assets/cbf99be4-4de1-4a4e-af64-994656273e16)
'Track_Destination_Lat' = endpoint of the track of a flight (further definition in sections below) - latitude in decimal format

'Track_Destination_Lon' = endpoint of the track of a flight (further definition in sections below) - longitude in decimal format

'Track_Destination_FL_Ft' = end flight level of the track of a flight - can be 'ground' or flight level (depending on the ADS-B antenna coverage - see possible cases in sections below)

'Track_Destination_DateTime_UTC' = end date and time of the track of a flight - UTC format

'Track_Destination_ApplicableAirports' = airport(s) in the vicinity of the endpoint of the track of a flight - airport ICAO code (further details and possible cases in sections below)

'Route_Validation_Based_on_Callsign' = to correct flights of which the ADS-B track was incomplete (see details in sections below), a callsign vs route lookup ensures a start/end airport are nevertheless available - for reference in addition to the raw data

_Note: sometimes the ADS-B antenna coverage is limited to the extent that the entire arrival or departure part of a flight was unavailable. In that case, those cells feature a '-' only._


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
For questions, please refer to my LinkedIn profile [Sebastiaan Menger - LinkedIn](https://de.linkedin.com/in/sebastiaanmenger)


# Upon Request
Contact me for enhanced datasets featuring:
- Filtered airport-specific cleaned flight schedules
- Aircraft seats
- Probable RWY used
- Corrected RWY times in case of incomplete tracks
- RWY times in local timezone/daylight saving time
- Plausible airport in case of incomplete tracks
- Ancillary data such as airline type (FSC, LCC, etc), alliance, etc - Analysis
- Calculation of rolling hour traffic/seats data - Example below:

![rollinghrplot_eham_example](https://github.com/user-attachments/assets/1972e76d-2a1b-4f35-9f70-d85467631c4a)


# Details - What are the Date/Times in the Datasets?
This concerns high-level/approximated RWY times in UTC, so lift-off time for departures and touchdown time for arrivals. This is generally reference to the first 'ground' entry for arrivals, and the last 'ground' entry for departures. However, there can also be cases with more limited ADS-B coverage, where the track does not start or stop at the airport:

![image](https://github.com/user-attachments/assets/6c5e04a3-3268-4d6f-91cc-9f1ab479028b)

For those cases, the beginning/end of the track has been selected as the time of the flight. For further implications, see section below.


# Details - Why are There Multiple Airports Listed for a Flight?
Similar to the section above, for those cases where the track does not start or stop at the airport, multiple airports in the vicinity of the first/last position of the ADS-B track have been listed as options.
To nevertheless determine the plausible airport of origin/destination, validation data from [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01) has been included to match the aircraft flight callsign with external route data.


# Details - Potential Duplicates
Occasionally, in case of flight tracks with large timegaps (1), potential GPS spoofing (2) or detours due to (e.g.) thunderstorms (3), the flight linking algorithm could introduce a limited amount of duplicate filghts. An example is the Frankfurt (FRA) to Dubai (DXB) route, where the ADS-B position reports feature large timegaps (hours), as well as potentially some GPS spoofing over Turkey / the Black Sea. This could sometimes make it difficult to determine what parts of the route belong together, or potentially a landing occurred in between (in an area without coverage).

In the 2024 datasets, on the mentioned route, this could worst case result in approx. 10% duplicate flights being created (concentration of track transmissions in the Dubai area and concentration of track transmissions over Turkey/Europe, occasionally being considered as separate flights).

![timegaps vs gps spoofing vs flight linking_v4](https://github.com/user-attachments/assets/25e8ae56-2823-4764-9e4f-07dd74c04fa5)

For the 2025-Q2 datasets and onwards, the algorithm has been tweaked to also check callsign matches of the transmission reports during the enroute phase, to reduce duplicates. On the aforementioned route DXB - FRA, this resulted in 95+% accurate flights.


# Details - Accuracy for General Aviation Flights
The algorithm to extract a flight route from ADS-B positon reports is primarily focussed on commercial flights. The applied airport lookup list (to assign an airport to the start/end of a flight) is focussed on airports for commercial operations. As a result, several regional small airfields for general aviation (GA) are not included. This implies that the accuracy of the assigned airport of origin/airport of destination can be lower for GA flights. In the example below, a flight actually departing from EDKS (which is not in the airport lookup list), would then be assigned 'EDLW' as the closest airport featured in the lookup list:

![Picture2](https://github.com/user-attachments/assets/e0dc6439-59f0-417b-a902-62125390e89d)

This only concerns some GA flights, which is not the main aim of the dataset given its focus on commercial flights. From 2025 onwards, for each flight the origin lat/lon as well as destination lat/lon has been included, so that manual assignment of a regional GA airfield becomes possible.

![image](https://github.com/user-attachments/assets/fb43b350-3193-4b35-ace4-6cb571e66c12)


# Details - How are Go-Arounds Considered?
In case of go-arounds/touch-and-go/balked landings, only the final touchdown is counted as touchdown time of the flight - again with commercial flights in mind.

![image](https://github.com/user-attachments/assets/96de9c02-a204-4d1e-8198-3cb0069e93e2)


# The Future
The more ADS-B receivers are added to the [adsb.lol initiative](https://github.com/adsblol/feed) through [ADSB.im software](https://adsb.im/home), the more accurate the derived flight schedules in this repository also become (accuracy of airport of origin/destination and pertaining RWY times).
