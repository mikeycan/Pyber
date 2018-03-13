# Pyber

Observable Trends:

1.	Although we have more Urban trips but they produce lower fares.  Mostly likely they are shorter distances.
2.	We have higher fares for rural passengers as they most likely cover greater distances. 
3.	Rural passengers make up the largest portion of our business.


# Dependencies
import csv
import pandas as pd
import numpy as np
import os
import matplotlib.pyplot as plt


#read ride data
csvpath = os.path.join('generated_data', 'ride_data.csv')
rider_data = pd.read_csv(csvpath)

csvpath2 = os.path.join('generated_data', 'city_data.csv')
city_data = pd.read_csv(csvpath2)

city_data.head()

city_rider_data = pd.merge(rider_data, city_data, on=["city","city"], how="left")
city_rider_data.head()


#Calculate the following
# Average Fare per City
# Total Number of Rides per City
# Total Number of Drivers per City
# City Type

urban_cities = city_rider_data[city_rider_data["type"] == "Urban"]
suburban_cities = city_rider_data[city_rider_data["type"] == "Suburban"]
rural_cities = city_rider_data[city_rider_data["type"] == "Rural"]

total_rides = (urban_cities['driver_count'])+(suburban_cities['driver_count'])+(rural_cities['driver_count'])
total_urban_rides = (urban_cities['driver_count'])
total_suburban_rides = (suburban_cities['driver_count'])
total_rural_rides = (rural_cities['driver_count'])


urban_ride_count = urban_cities.groupby(["city"]).count()["ride_id"]
urban_avg_fare = urban_cities.groupby(["city"]).mean()["fare"]
urban_driver_count = urban_cities.groupby(["city"]).count()["driver_count"]

suburban_ride_count = suburban_cities.groupby(["city"]).count()["ride_id"]
suburban_avg_fare = suburban_cities.groupby(["city"]).mean()["fare"]
suburban_driver_count = suburban_cities.groupby(["city"]).count()["driver_count"]

rural_ride_count = rural_cities.groupby(["city"]).count()["ride_id"]
rural_avg_fare = rural_cities.groupby(["city"]).mean()["fare"]
rural_driver_count = rural_cities.groupby(["city"]).count()["driver_count"]

#scatter plots

urban_plot = plt.scatter(urban_ride_count,urban_avg_fare, s = total_urban_rides, c = 'lightcoral')
suburban_plot = plt.scatter(suburban_ride_count, suburban_avg_fare, s = total_suburban_rides, c = 'lightskyblue')
rural_plot = plt.scatter(rural_ride_count,rural_avg_fare, s = total_rural_rides, c = 'gold')

plt.style.use('seaborn-dark')
plt.title('Pyber Ride Sharing Data')
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')
plt.legend(['Urban','Rural','Suburban'],title = 'City Types')

plt.show()

#% of Total Fares by City Type
cities_totals_fare= city_rider_data.groupby(["type"]).sum()["fare"]

colors = ["gold", "lightcoral", "lightskyblue"]
explode = (0.1, 0.1, 0)
plt.pie(cities_totals_fare, explode=explode, colors=colors, labels=city_rider_data['type'].unique(),
        autopct='%.2f', shadow=True, startangle=140)
plt.title ('% of Total Fares by City Type')
fig = plt.gcf()
fig.set_size_inches(6,6)
plt.show()

#% of Total Rides by City Type
cities_totals_ride= city_rider_data.groupby(["type"]).count()["ride_id"]
colors = ["gold", "lightcoral", "lightskyblue"]
explode = (0.1, 0.1, 0)
plt.pie(cities_totals_ride, explode=explode, colors=colors, labels=city_rider_data['type'].unique(),
        autopct='%.2f', shadow=True, startangle=140)
plt.title ('% of Total Rides by City Type')
fig = plt.gcf()
fig.set_size_inches(6,6)
plt.show()

#% of Total Drivers by City Type
cities_totals_driver= city_data.groupby(["type"]).sum()["driver_count"]
colors = ["gold", "lightcoral", "lightskyblue"]
explode = (0.1, 0.1, 0)
plt.pie(cities_totals_driver, explode=explode, colors=colors, labels=city_data['type'].unique(),
        autopct='%.2f', shadow=True, startangle=140)
plt.title ('% of Total Rides by City Type')
fig = plt.gcf()
fig.set_size_inches(6,6)
plt.show()
