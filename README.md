# Autolib-Project
Independent Project w4
Data Preparation
Data was prepared for use using Python. I used the whole dataset and some of the columns were dropped as they did not add any value to the analysis. The following were undertaken when cleaning the data:
Dropping unnecessary columns:
#dropping the rows that are unnecessary 
df.drop(['Address','Cars','Charge Slots','Charging Status','Displayed comment','Scheduled at', 'Slots','Geo point'], axis = 1, inplace = True)
The remaining columns were:
Index(['Bluecar counter', 'Utilib counter', 'Utilib 1.4 counter', 'City', 'ID', 'Kind', 'Postal code', 'Public name', 'Rental status', 'Station type', 'Status', 'Subscription status', 'year', 'month', 'day', 'hour', 'minute'],  dtype='object')
The columns that remained were all converted to lower case format for ease of referencing while doing the analysis using the following code:
#converting column names to lower case
df.columns = map(str.lower, df.columns)
All column names that has spaces between were all replaced with an underscore using the following code for uniformity among the column names.
df.columns = df.columns.str.strip().str.replace(' ','_')
The year, month and day columns were then combined to create a new column date that contained all the date information.
parking['date'] = pd.to_datetime((parking.year*10000+parking.month*100+parking.day).apply(str),format='%Y%m%d')
The hour and minute columns were also combined to create a new column time.
parking['time'] = parking.hour.str.cat([parking.minute], sep = ':')
A new Csv was then created ‘cars.csv’ that contained the cleaned dataset that aided in working with a sizeable data with valuable information.
Data Analysis
In order to get the most popular hour of the day in the city of Paris for picking up the shared Bluecars, I selected only the column where city was Paris in order to do the analysis by subsetting the dataframe.
#subsetting the data frame to only select only stations in Paris
pariscity = cars[(cars.city == 'Paris')]

	bluecar_counter	utilib_counter	utilib_1.4_counter	city	id	kind	postal_code	public_name	rental_status	station_type	status	subscription_status	day	hour	minute	date	time
0	3	1	0	Paris	paris-vulpian-24	STATION	75013	Paris/Vulpian/24	operational	station	ok	nonexistent	1	0	0	2018-04-01	0:0
1	0	0	0	Paris	paris-richardlenoir-8	STATION	75011	Paris/Richard Lenoir/8	operational	station	ok	nonexistent	1	0	0	2018-04-01	0:0
2	3	0	0	Paris	paris-charlesbossut-4	STATION	75012	Paris/Charles Bossut/4	operational	station	ok	nonexistent	1	0	0	2018-04-01	0:0
3	4	0	0	Paris	paris-claudevellefaux-3	STATION	75010	Paris/Claude Vellefaux/3	operational	station	ok	nonexistent	1	0	0	2018-04-01	0:0

To get the most popular hour in the City of Paris the following code was used:
#most popular hour of the day for picking shared electric blue car in Paris
pariscity.groupby(['hour'])['bluecar_counter'].sum()
#print(x)
#.sort_values(ascending = False)

The most popular hour was got by getting the difference between the hours and most popular was hour 23 which had the most negative number of cars.
x.diff().sort_values(ascending = False)
8    -47931.0
23   -54699.0

The most popular hour for returning cars was Hour 21 which had the most positive number of cars:
21    66986.0
2     64544.0

This is also shown in the bar graph below:
 
The most popular station in Paris was:

pariscity.groupby(['id'])['Car_Total'].sum().sort_values(ascending = False).head()
id
paris-portesaintcloud-parking      71800
paris-versaillesreynaud-parking    67970
paris-picpusnation-parking         64766
paris-gouvionmaillot-parking       63415
paris-lacordaire-56                60294


The most popular station and most picking hour in Paris in terms of the total number of cars
pariscity.groupby(['id','hour'])['Car_Total'].sum().sort_values(ascending = False).head(15)
id                               hour
paris-portesaintcloud-parking    12      3238
                                 11      3236
                                 14      3216
                                 18      3213
                                 7       3196
                                 19      3194
                                 10      3190
                                 13      3178
                                 15      3172
                                 16      3162
                                 6       3159
                                 17      3141
paris-versaillesreynaud-parking  5       3106
                                 2       3092
                                 4       3090

The most popular postal code for picking up blue cars
cars.groupby(['postal_code','hour'])['bluecar_counter'].sum().sort_values(ascending = False).head()

postal_code  hour
75015        6       92407
             5       91494
             4       89615
             3       89528
             7       89175
Name: bluecar_counter, dtype: int64

The most popular station for the company was:
pariscity.groupby(['id','postal_code'])['Car_Total'].sum().sort_values(ascending = False).head()
id                               postal_code
paris-portesaintcloud-parking    75016          71800
paris-versaillesreynaud-parking  75016          67970
paris-picpusnation-parking       75012          64766
paris-gouvionmaillot-parking     75017          63415
paris-lacordaire-56              75015          60294

The most popular station, Paris – Portesaintcloud- parking which is postal code 75016 does not belong to the most popular postal code which is 75015. They differ.
The results change when one uses Utilib and Utilib 1.4. With Utilib, the popular station is colombes-gounoud-1 of postal code 92700.
id                        postal_code  hour
colombes-gounod-1         92700        11      464
                                       10      459
                                       12      447
nanterre-julesquentin-33  92000        17      429
colombes-gounod-1         92700        13      429

With Utilib 1.4, the popular station is sevres-granderue-123 of postal code 92310.
id                      postal_code  hour
sevres-granderue-123    92310        7       546
drancy-rogersalengro-3  93700        11      545
                                     6       545
                                     2       545
sevres-granderue-123    92310        3       544
Name: utilib_1.4_counter, dtype: int64

