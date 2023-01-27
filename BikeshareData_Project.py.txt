import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    global city
    city = input("Specify from which city do you like to explore the data? Chicago,New Yourk city or Washington?:").lower()
    while True:
            city == CITY_DATA[city]
            break
            
    # TO DO: get user input for month (all, january, february, ... , june)
    month = input("Which month? You can choose all or any specific months january, february, ..., june:").lower()

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    day = input("Which day of the week? You can choose all or specify a one day monday, tuesday, ..., sunday:").lower()
    
    print('-'*40)
    return city, month, day


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

    
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.weekday_name

    
    if month != 'all':
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1

     
        df = df[df['month'] == month]

    
    if day != 'all':
        
        df = df[df['day_of_week'] == day.title()]

    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    most_common_month = df['month'].mode()[0]
    print(most_common_month)

    # TO DO: display the most common day of week
    most_common_day = df['day_of_week'].mode()[0]
    print(most_common_day)

    # TO DO: display the most common start hour
    most_common_start_hour = df['Start Time'].mode()
    print(most_common_start_hour)
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    most_used_start_station = df['Start Station'].mode()
    print(most_used_start_station)
    # TO DO: display most commonly used end station
    most_used_end_station = df['End Station'].mode()
    print(most_used_end_station)
    # TO DO: display most frequent combination of start station and end station trip
    combination = df.groupby(df['Start Station'])['End Station']
    print(combination)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_duration = df['Trip Duration'].sum()
    print(total_duration)
    # TO DO: display mean travel time
    average_duration = df['Trip Duration'].mean()
    print(average_duration)
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()
    
    # TO DO: Display counts of user types
    user_type = df['User Type'].value_counts()
    print(user_type)
    # TO DO: Display counts of gender
    if city == 'washington':
        print("Washington city data does not include gender.")
    else:
        user_gender = df['Gender'].value_counts()
        print(user_gender)
    # TO DO: Display earliest, most recent, and most common year of birth
    if city == 'washington':
        print("Washington city data does not include birth year.")
    else:
        earliest_year = df['Birth Year'].min()
        most_recent_year = df['Birth Year'].max()
        common_year = df['Birth Year'].mode()
        print(earliest_year)
        print(most_recent_year)
        print(common_year)
   
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
            
        show_data = input('\nWould you like to see first 5 lines of the data? Enter yes or no.\n')
        if show_data.lower() == 'yes':
            print(df.head())
            
       
        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break

if __name__ == "__main__":
	main()
