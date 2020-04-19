import time
import pandas as pd
import numpy as np


CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }
			  
def get_filters():
    
    lista_city = ['chicago', 'washington', 'new york city']
    lista_month = ['january', 'february', 'march', 'april', 'may', 'june', 'all']
    lista_day = ['monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday', 'sunday', 'all']

    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
  
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    x = 0 
    while x == 0 :  
        city = input("Input a city: ")
        city = city.lower()
        if city in lista_city:
            x = 1        
    
    # TO DO: get user input for month (all, january, february, ... , june)
    y = 0 
    while y == 0:
        month = input("Input a months: ")
        month = month.lower()
        if month in lista_month:
            y = 1 
       
    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    z = 0 
    while z == 0 :
        day = input("Input a day: ")
        day = day.lower()
        if day in lista_day:
            z = 1 


    print('-'*40)
    return city, month, day

# =====================================================================================================================

def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    
    df = pd.read_csv(CITY_DATA[city])
    df['Start Time'] = pd.to_datetime(df['Start Time'])

    # extract month and day of week from Start Time to create new columns
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.weekday_name

    # filter by month if applicable
    if month != 'all':
        # use the index of the months list to get the corresponding int
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1

        # filter by month to create the new dataframe
        df = df[df['month'] == month]

    # filter by day of week if applicable
    if day != 'all':
        # filter by day of week to create the new dataframe
        df = df[df['day_of_week'] == day.title()]

    return df

 
# =========================================================================
def time_stats(df):
    #Displays statistics on the most frequent times of travel.

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()
#    df = pd.read_csv(df)
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    
	# TO DO: display the most common month
	# load data file into a dataframe

    #print(df['Start Time'])
	# extract hour from the Start Time column to create an hour column
    df['month'] = df['Start Time'].dt.month

	# find the most popular hour
    popular_month = df['month'].mode()[0]
    print('Most common month:', popular_month)


	# TO DO: display the most common day of week

    #print(df['Start Time'])
	# extract hour from the Start Time column to create an hour column
    df['dayofweek'] = df['Start Time'].dt.dayofweek

	# find the most popular hour
    popular_dayofweek = df['dayofweek'].mode()[0]

    print('Most common day of week:', popular_dayofweek)


    # TO DO: display the most common start hour

    #print(df['Start Time'])
	# extract hour from the Start Time column to create an hour column
    df['hour'] = df['Start Time'].dt.hour

	# find the most popular hour	
    popular_hour = df['hour'].mode()[0]

    print('Most common Start Hour:', popular_hour)



    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

# =============================================================================	
	
def station_stats(df):
    #Displays statistics on the most popular stations and trip.

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()
   # df = pd.read_csv(df)
    
    # TO DO: display most commonly used start station

    # find the most popular hour
    popular_start_station = df['Start Station'].mode()[0]

    print('Most commonly used start station:', popular_start_station)

    # TO DO: display most commonly used end station

    # find the most popular hour
    popular_end_station = df['End Station'].mode()[0]

    print('Most commonly used end station:', popular_end_station)


    # TO DO: display most frequent combination of start station and end station trip

    df['combination_station'] = df["Start Station"] + ' ' + '-' + ' ' + df["End Station"]

    # find the most popular hour
    popular_combination_station = df['combination_station'].mode()[0]

    print('Most frequent combination of start station and end station trip:', popular_combination_station)
    

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

	
# =============================================================================

def trip_duration_stats(df):
    #Displays statistics on the total and average trip duration.

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time

    # load data file into a dataframe
   # df = pd.read_csv(df)
    var1 = df['Trip Duration'].sum()
    
    print('Display total travel time:', var1)

    # TO DO: display mean travel time
    
    var2 = df['Trip Duration'].mean()
    print('Display mean travel time:', var2)
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)
	
	

# =============================================================================
def user_stats(df):
    #Displays statistics on bikeshare users

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types

    # load data file into a dataframe
    #df = pd.read_csv(df)

    # print value counts for each user type
    user_types = df['User Type'].value_counts()
    print(user_types)
    
    # TO DO: Display counts of gender
    if 'Gender' in df.columns:
        gender = df['Gender'].value_counts()
        print(gender)
    else:
        print('No data for gender')

        
        
    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' in df.columns:
        earliest_year = df['Birth Year'].min()
        print('Earliest year of birth:' , earliest_year)
    
        recent_year = df['Birth Year'].max()
        print('Recent year of birth:' , recent_year)
    
        birth_year = df['Birth Year'].value_counts()
        a = 0
        while a == 0:
            raw_data = input('\nDo you want to see raw data for the most common year of birth? Enter yes or no.\n')
            if raw_data.lower() == 'yes':
                print('Most common year of birth:' , birth_year.iloc[:5])
                a = 1
            elif raw_data.lower() == 'no':
                print('Most common year of birth:' , birth_year)
                a = 1
            else: 
                a = 0       
    
    else:
        print('No data for birth year')
             
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40) 

# =============================================================================
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
    main()
