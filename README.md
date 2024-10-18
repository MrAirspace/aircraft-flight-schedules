# Aircraft Flight Logs

WORK IN PROGRESS - Update expected by the end of October 2024

**_High-level_** flights per aircraft, extracted from ADS-B position reports of the ADSBlol initiative (https://github.com/adsblol).
Published per quarter of a year, starting from 2024 onwards. Covers all flights globally, as long as within coverage of the ADSBlol initiative.

![EHAM ADSB Tracks to Flight Logs_spaced2](https://github.com/user-attachments/assets/c5f20d82-a135-4389-a926-6f21b0f47f79)


# Where to Get the Datasets?
See the 'releases' section of this repository for a tar/zip file with the flights per aircraft, per quarter of a year
Can you link this to source github page?


# Data Timeframes
The data is published per quarter of a year. The quarters feature some overlap to ensure no flights are incomplete (cut in half).


# Data Enrichment - Against Limited Coverage in Certain Areas
Given potentially limited ADS-B reception coverage of the ADSBlol initiative in certain continents, some aircraft tracks start after the airport of origin or end before the airport of destination. For those cases, the flights data has been enhanced by looking up the aircraft flight callsign and matching it with the open-source aircraft callsign vs route dataset of Andrew Whewell (https://github.com/vradarserver/standing-data/tree/main/routes/schema-01). Kindly note the departure/arrival times have not been updated accordingly in the basic dataset provided here.


# Data Enrichment - GPS Spoofing
Given ADS-B transmissions simply sending location data, wrong location data as a result of GPS spoofing can also be transmitted. Once more, the added column with callsign vs route lookup allows to filter out those flights where aircraft emitted wrong position data.


# How to Extract Flights for a Certain Airport?
Since the datasets contain flights per aircraft (per quarter of a year), if you want to extract historic flights to/from a certain airport, you need to extract those from all aircraft flight files - matching your airport in the aircraft its origin/destination columns. If you need support with this, please get in touch. I can also offer enhanced datasets.


# Detailed Data and Support
If you require specfic data or analyses, you can contact me, details in my github profile. More detailed datasets available upon request include updated departure/arrival times for those cases where the aircraft track was not complete (1), on/off-block instead of RWY times (2), used RWY per flight (3), among other data. Furthermore, advanced passenger estimates per flight (4) taking into consideration the type of flight, time of day, etc can be provided upon request.


# Data Coverage
_Status Q2 2024_
![image](https://github.com/user-attachments/assets/92117619-ecc2-48f3-bc73-07407cca4445)
Number of receivers/antennas of ADSBlol initiative (image above)

![image](https://github.com/user-attachments/assets/b96a126c-00aa-4076-9882-f5a84669eb13)
Aircraft coverage of ADSBlol initiative. Time of day ~13:00 UTC to have reasonable ops in all continents - no midnight situation in major markets (image above)


# Flights Extracts - Validation Case Study
Given the fact that ADSBlol coverage improves regularly, validation is a never finished task, especially for airports/flights all over the world.
So far, AMS/EHAM flights have been analysed and results:

_Validation case study_
AMS/EHAM reference day 2024-06-14

- Number of (commercial!) flights extracted from ADS-B data vs # flights from real AMS schedule --> significantly close, within 5% error margin

- Airline representation --> significantly close, within 5% error margin

- Destination/origin of a flight accurate 73% of time purely based on ADS-B track data, improved to 95+% by using callsign vs route lookup


# License
Please use in line with license defined in this repository. No guarantee, liability, warranty. All open-source. Free to use.


# Issues Reporting
In case of data inaccuracies, please use the 'issues' tab of this repository.


# Details - What are the Date/Times in the Datasets?
This concerns RWY times, so lift-off time for departures and touchdown times for arrivals.


# Details - How are Go-Arounds Considered?
In case of go-arounds/touch-and-go/balked landings, only the final touchdown is counted as touchdown time of the flight (with commercial flights in mind)

![image](https://github.com/user-attachments/assets/96de9c02-a204-4d1e-8198-3cb0069e93e2)
