---
layout: post
date: 2019-01-26 19:22
title: Making an program similar to @year_progress 
---





This program takes a datetime string and returns the percentage of the time elapsed.

```python
months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October",
"November","December"]
month_days = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30,31]

def parseDate(currentDate):
    parts = currentDate.split(",")
    month,day = parts[0].split(" ")
    year,time = parts[1].split(" ")[1:]
    return (month,day,int(year),time)

def calculatedays(month,day,year):
    total_days = 0
    ind = months.index(month)-1
    for i in range(ind+1):
        total_days += month_days[i]
    if (((year % 4 == 0) and (year % 100 != 0)) or (year % 400 == 0)) and ind>0:
        total_days += 1
    total_days += int(day)-1
    return total_days
def yearProgress(currentDate):
    month,day,year,time = parseDate(currentDate)
    h,m = map(int,time.split(":"))
    #print(calculatedays(month,day,year)*24*60+h*60+m)
    total_time_in_minutes = 366*24*60 if (((year % 4 == 0) and (year % 100 != 0)) or (year % 400 == 0)) else 365*24*60 
    total_time_passed = calculatedays(month,day,year)*24*60+h*60+m
    print((total_time_passed/total_time_in_minutes)*100)

yearProgress("May 10, 2021 00:31")
yearProgress("January 31, 1900 00:47")

```
