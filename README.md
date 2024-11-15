# Aircraft Flight Logs

Datasets featuring global, **_high-level_** flight schedules per aircraft, extracted from ADS-B position reports.

Published per quarter of a year, starting from 2024 onwards. Covers all flights as long as within coverage of the [ADSBlol](https://github.com/adsblol) initiative.

![382481228-bdabfdb8-af16-42a1-8263-1db46fa8f8b9](https://github.com/user-attachments/assets/e8003711-2370-4604-8db9-42fdcb9a35b8)


# Data Sources
1) This project uses the ADS-B data from the [ADSBlol](https://github.com/adsblol) initiative. Consider supporting their great project.
2) This project uses validation data from [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01) to check extracted routes with additional route data (based on aircraft callsign). Again, consider supporting this initiative.


# Where to Get the Aircraft Flight Logs Datasets?
See the [Releases](https://github.com/MrAirspace/aircraft-flight-logs/releases) section of this repository for a parquet file with the flights per aircraft, per quarter of a year.


# How to Load the Dataset?
The parquet filetype has been selected to keep flights data manageable in terms of size and processing/loading times. Each quarter features approx. 10-12+ million flights and ~500,000 aircraft, which in csv format would total approx. 3 GB. Hence the selection of a parquet filetype, which stays far below 1 GB.
Loading a parquet file is very straightforward with python:

`df = pandas.read_parquet('2024_Q1.parquet')`

Furthermore, to check the parquet dataset without python, you can use tools like [ParquetViewer](https://github.com/mukunku/ParquetViewer) which feature a user interface/GUI and can be installed on Windows as exe.


# Data Timeframes
The data is published per quarter of a year. The 4 quarters of each year feature some overlap to ensure no flights are incomplete (not cut in half).


# Data Enrichment - Against Limited Coverage in Certain Areas
Given potentially limited ADS-B reception coverage of the ADSBlol initiative in certain continents, some aircraft tracks start after the airport of origin or end before the airport of destination. For those cases, the flights data has been enhanced by looking up the aircraft flight callsign and matching it with the open-source aircraft callsign vs route dataset of [vradarserver/Andrew Whewell](https://github.com/vradarserver/standing-data/tree/main/routes/schema-01).


# Data Enrichment - GPS Spoofing
Given ADS-B transmissions simply sending location data, wrong location data as a result of GPS spoofing can also be transmitted. Once more, the added column with callsign vs route lookup allows to filter out those flights where aircraft emitted wrong position data.


# Data Coverage
_Status Q2 2024_
![image](https://github.com/user-attachments/assets/92117619-ecc2-48f3-bc73-07407cca4445)
Number of receivers/antennas of ADSBlol initiative (image above)

![image](https://github.com/user-attachments/assets/b96a126c-00aa-4076-9882-f5a84669eb13)
Aircraft coverage of ADSBlol initiative. Time of day ~13:00 UTC to have reasonable ops in all continents - no midnight situation in major markets (image above)


# Flights Extracts - Validation Case Study
Given the fact that ADSBlol coverage improves regularly, validation of the extracted flights is a never finished task, especially given the global scope.

At present, each quarter of extracted flights features approx. 10-12+ million flights and ~500,000 aircraft.

_Validation Case Study - AMS/EHAM Reference Day 2024-06-14_

- Number of (commercial!) flights extracted from ADS-B data vs # flights from AMS schedule --> significantly close, within 5% error margin

- Airline representation --> significantly close, within 5% error margin

- Destination/origin of a flight accurate 73% of time purely based on ADS-B track data, improved to 95+% by using callsign vs route lookup


# License
Please use in line with the license defined in this repository. No guarantee, no liability, no warranty. All open-source.


# Details - What are the Date/Times in the Datasets?
This concerns RWY times, so lift-off time for departures and touchdown time for arrivals.

However, there can also be cases with more limited ADS-B coverage, where the track does not start or stop at the airport:
![image](https://github.com/user-attachments/assets/6c5e04a3-3268-4d6f-91cc-9f1ab479028b)

For those cases, the beginning/end of the track has been selected as the time of the flight.


# Details - Why are There Multiple Airports Listed for a Flight?
Similar to the section above, for those cases where the track does not start or stop at the airport, multiple airports in the vicinity of the first/last position of the ADS-B track have been listed as options.


# Details - How are Go-Arounds Considered?
In case of go-arounds/touch-and-go/balked landings, only the final touchdown is counted as touchdown time of the flight (with commercial flights in mind).

![image](https://github.com/user-attachments/assets/96de9c02-a204-4d1e-8198-3cb0069e93e2)
