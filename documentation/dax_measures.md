# DAX Measures — Hotel Management Dashboard
This document lists all DAX measures used in the Power BI report, along with a short explanation of what each one calculates.

## Total Revenue
Calculates total revenue by multiplying the average daily rate by total nights stayed, per booking.

Total Revenue = SUMX(hotel_bookings, hotel_bookings[adr] * (hotel_bookings[stays_in_week_nights] + hotel_bookings[stays_in_weekend_nights]))

## Total Occupied Rooms
Counts bookings that were not cancelled.

Total Occupied Rooms = CALCULATE(COUNTROWS(hotel_bookings), hotel_bookings[is_canceled] = 0)

## Total Cancelled Bookings
Counts bookings that were cancelled.

Total Cancelled Bookings = CALCULATE(COUNTROWS(hotel_bookings), hotel_bookings[is_canceled] = 1)

## Total Days of Booking
Sum of all week nights and weekend nights, excluding cancelled bookings.

Total Days of Booking = CALCULATE(SUM(hotel_bookings[stays_in_week_nights]) + SUM(hotel_bookings[stays_in_weekend_nights]),

## Total Weekend Nights
Sum of weekend nights stayed, excluding cancelled bookings.

Total Weekend Nights = CALCULATE(SUM(hotel_bookings[stays_in_weekend_nights]), hotel_bookings[is_canceled] = 0)

## Total Weekday Nights
Sum of weekday (week) nights stayed, excluding cancelled bookings.

Total Weekday Nights = CALCULATE(SUM(hotel_bookings[stays_in_week_nights]), hotel_bookings[is_canceled] = 0)
hotel_bookings[is_canceled] = 0)

## Average Daily Rate (ADR)
Average rate charged per night across all bookings.

Average Daily Rate = AVERAGE(hotel_bookings[adr])

## Week Nights Revenue
Revenue generated specifically from weekday night stays.

Week Nights Rev = SUMX(hotel_bookings, hotel_bookings[adr] * hotel_bookings[stays_in_week_nights])

## Weekend Nights Revenue
Revenue generated specifically from weekend night stays.

Weekend Nights Rev = SUMX(hotel_bookings, hotel_bookings[adr] * hotel_bookings[stays_in_weekend_nights])

## Occupancy Rate
Percentage of total bookings that resulted in an occupied room (not cancelled).

Occupancy Rate = DIVIDE([Total Occupied Rooms], COUNTROWS(hotel_bookings))

## RevPAR (Revenue Per Available Room)
Revenue divided by number of occupied rooms.

RevPAR = DIVIDE([Total Revenue], [Total Occupied Rooms])

## Avg Days of Stay
Average total nights (week + weekend) stayed per booking.

Avg Days of Stay = AVERAGEX(hotel_bookings, hotel_bookings[stays_in_week_nights] + hotel_bookings[stays_in_weekend_nights])

## Date Table
A dedicated calendar table built to support time intelligence and enable clean relationships with the fact table.

DateTable = CALENDAR(MIN(hotel_bookings[Arrival Date]), MAX(hotel_bookings[Arrival Date]))

### Supporting Date Table columns

Year = YEAR(DateTable[Date])

Month = FORMAT(DateTable[Date], "MMMM")

MonthNumber = MONTH(DateTable[Date])

Week = WEEKNUM(DateTable[Date])
