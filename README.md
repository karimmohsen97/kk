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
    while true:
        city = input ('choose the city (chicago, new york city or washinton)')
        if city not in CITY_DATA:
            print('wrong city')
        else:
            break


    # TO DO: get user input for month (all, january, february, ... , june)
    while true:
        month = input('choose the month from (all, january, february, ... , june))
        all_months = ['januray', 'february', 'march', 'april', 'may', 'june']
        if month != all_months:
            print('wrong')
        else:
            break
                      


    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    while true:
        day = input('choose the day (all, monday, tuesday, ... sunday)
        days = ['monday','tuesday','wednesday','thursday','friday','saturday','sunday']
        if day != days:
            print ('wrong choose again')
        else :
            break    



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
    df['month'] =df['Start Time'].dt.month
    df['day_of_week']=df['Start Time'].dt.weekday_name
    df['hour']=df['Start Time'].dt.hour
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
    print ('the most common month is' , most_common_month)

    # TO DO: display the most common day of week
    most_common_day = df['day_of_week'].mode()[0]
    print('the most common day of week is', most_common_day)

    # TO DO: display the most common start hour
    most_common_hour = df['hour'].mode()[0]
    print('the most common start hour is', most_common_hour)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    common_end = df['end station'].mode()[0]
    print('most commonly used start station is', common_end)


    # TO DO: display most commonly used end station
    common_end = df['end station'].mode()[0]
    print('most commonly used end station is', common_end)


    # TO DO: display most frequent combination of start station and end station trip
    common_start_end = (df['start station']+ '---' + df['end station'].mode()[0]
    print('most frequent start & end station are', common_start_end)


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_time = df['trip duration'].sum()
    print('total travel time is', total_time)               
    # TO DO: display mean travel time
    mean_time = df['trip duration'].mean()
    print('mean travel time is', mean_time)
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    print('counts of user types', df['user type'].value_counts())


    # TO DO: Display counts of gender
    if city != washington:
    print('counts of gender', df['Gender'].value_counts())


    # TO DO: Display earliest, most recent, and most common year of birth
    print('earlist year of birth is', df['Birth Year'].min())
    print('the most recent year of birth is', df['Birth Year'].max())
    print('most common year of birth is', df['Birth Year'].mode()[0])


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

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
