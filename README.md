# aircraft-flight-logs
**_High-level_** flights per aircraft - wip

All airports of the world, as long as within coverage of the ADSBlol initiative

Can you link this to source github page?

If you require specfic data or analyses, please contact me.

![image](https://github.com/user-attachments/assets/daa94716-cab0-4d94-beed-233c0b44c4a6)

In case of go-arounds/touch-and-go/balked landing, only the final touchdown is counted as touchdown time. The database is composed with commercial flights in mind only:

![image](https://github.com/user-attachments/assets/96de9c02-a204-4d1e-8198-3cb0069e93e2)


Validation: given the fact that ADSBlol coverage improves regularly, validation is a never finished task, especially for airports/flights all over the world.
So far, AMS/EHAM flights have been analysed and results:

Validation case study:
AMS/EHAM reference day 2024-06-14

Number of flights extracted from ADSB data vs # flights from real AMS schedule --> significantly close, within 5% error margin

Airline representation --> significantly close, within 5% error margin
Number of KLM flights 99.5% included in dataset on respective day

Destination/origin of a flight inaccurate 27% of time, to be updated.

Data includes first and last coordinates points, so if desired you can assign the most likely RWY --> image?

Status Q2 2024:
![image](https://github.com/user-attachments/assets/92117619-ecc2-48f3-bc73-07407cca4445)
![image](https://github.com/user-attachments/assets/b96a126c-00aa-4076-9882-f5a84669eb13)
(time roughly chosen to have reasonable ops in all continents - no midnight situation in major markets)

Please use in line with license defined in this repository. No guarantee, liability, warranty

Note the database contains flights per aircraft (per season). If you want to extract historic flights to/from a certain airport, you need to extract those from all a/c files - based on your airport in the origin/destination. If you need support with this, please get in touch.
