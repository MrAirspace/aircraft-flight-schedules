# Aircraft Flight Logs (aircraft-flight-logs)

WORK IN PROGRESS - Update expected by the end of October 2024

**_High-level_** flights per aircraft, extracted from ADS-B position reports of the ADSBlol initiative (https://github.com/adsblol).
Published per quarter of a year, starting from 2024 onwards. Covers all flights globally, as long as within coverage of the ADSBlol initiative.

Since the datasets contain flights per aircraft (per season), if you want to extract historic flights to/from a certain airport, you need to extract those from all aircraft flight files - based on your airport listed in the aircraft its origin/destination. If you need support with this, please get in touch. I can also offer enhanced datasets.

![image](https://github.com/user-attachments/assets/daa94716-cab0-4d94-beed-233c0b44c4a6)


# Where to Get the Data?
See releases for a tar/zip file with the flights per aircraft, per season
Can you link this to source github page?


# Data Enrichment
Given potentially limited ADSB-B reception coverage of the ADSBlol initiative in certain continents, some aircraft tracks start after the airport of origin or end before the airport of destination. For those cases, the flights data has been enhanced by looking up the aircraft flight callsign and matching it with the open-source aircraft callsign vs route database of Andrew Whewell (https://github.com/vradarserver/standing-data/tree/main/routes/schema-01). Kindly note the departure/arrival times have not been updated accordingly in the basic dataset provided here.


# Data Timeframes
The data is published per quarter of a year. The quarters feature some overlap to ensure no flights are incomplete (cut in half).


# Detailed Data and Support
If you require specfic data or analyses, please contact me. More detailed datasets available upon request include updated departure/arrival times for those cases where the aircraft track was not complete (1), in/off-block instead of RWY times (2), used RWY per flight (3), among other data. Furthermore, advanced passenger estimates per flight (4) taking into consideration the type of flight, time of day, etc can be provided upon request.

www.ddfsgenerator.com


# Validation - Diverse
In case of go-arounds/touch-and-go/balked landing, only the final touchdown is counted as touchdown time (with only commercial flights in mind):

![image](https://github.com/user-attachments/assets/96de9c02-a204-4d1e-8198-3cb0069e93e2)

Given the fact that ADSBlol coverage improves regularly, validation is a never finished task, especially for airports/flights all over the world.
So far, AMS/EHAM flights have been analysed and results:

_Validation case study_
AMS/EHAM reference day 2024-06-14

- Number of flights extracted from ADSB data vs # flights from real AMS schedule --> significantly close, within 5% error margin

- Airline representation --> significantly close, within 5% error margin

- Number of KLM flights 99.5% included in dataset on respective day

- Destination/origin of a flight inaccurate 27% of time, to be updated. Hence, per callsign, the 

Data includes first and last coordinates points, so if desired you can assign the most likely RWY --> image?

_Status Q2 2024_
![image](https://github.com/user-attachments/assets/92117619-ecc2-48f3-bc73-07407cca4445)
![image](https://github.com/user-attachments/assets/b96a126c-00aa-4076-9882-f5a84669eb13)
(time roughly chosen to have reasonable ops in all continents - no midnight situation in major markets)

# License
Please use in line with license defined in this repository. No guarantee, liability, warranty. But free to use for whichever purpose.

