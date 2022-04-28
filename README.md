# Assignment2

import pandas as pd
import matplotlib
import sys
import plotly.express as plt



def extract_year_country(df):
    '''

    :param df: main dataframe
    :return: seperate year and country dataframe
    '''

    extract_years = list(df.columns)[4:]
    years_dataframe = pd.DataFrame(extract_years, columns=['Years'])
    extract_countries = df['Country Name'].drop_duplicates().values
    countries_dataframe = pd.DataFrame(extract_countries, columns=['Country'])
    return years_dataframe,countries_dataframe


def preprocess_df(df):
    '''

    :param df: original input csv dataframe
    :return: removes duplicates,nans etc and gives a proper dataframe
    '''

    remove_nan_df = df.iloc[3:, :]
    main_df = remove_nan_df.iloc[1: , :]
    return main_df


def analyse_population(main_df):
    '''
    :param main_df: main dataframe with all data
    :plots: plots a line chart to compare population in urban in 1960 for few countries
    '''

    df_1960 = main_df[['Indicator Name', 'Country Name', 1960.0]].where(
        main_df['Country Name'].isin(['Bangladesh', 'Ethiopia', 'Argentina', 'United States', ''])).dropna()
    urban_pop_df = df_1960.where(df_1960[
                                     'Indicator Name'] == 'Population in urban agglomerations of more than 1 million (% of total population)').dropna()

    plt.line(urban_pop_df, y=1960, x="Country Name", title="urban Population")


def analyse_agriculture_land(main_df):
    '''

    :param main_df: main dataframe with all data
    :plots: bar chart to compare agri land in various countries in 1970
    '''
    df_1970 = main_df[['Indicator Name', 'Country Name', 1970.0]].where(
        main_df['Country Name'].isin(['Bangladesh', 'Ethiopia', 'Argentina', 'United States', ''])).dropna()
    agr_df = df_1970.where(df_1970['Indicator Name'] == 'Agricultural land (% of land area)').dropna()

    plt.bar(agr_df, y=1970, x="Country Name", title="Agricultural land")


def analyse_co2(main_df):
    '''

    :param main_df: main dataframe with all data
    :plots: A scatter plot to compare co2 emissions in various countries in 1980
    '''
    df_1980 = main_df[['Indicator Name', 'Country Name', 1980.0]].where(
        main_df['Country Name'].isin(['Bangladesh', 'Ethiopia', 'Argentina', 'United States', ''])).dropna()
    co2_df = df_1980.where(df_1980['Indicator Name'] == 'CO2 emissions from solid fuel consumption (kt)').dropna()

    plt.scatter(co2_df, y=1980, x="Country Name", title="CO2 emissions")


def analyse_trends(main_df):
    '''

    :param main_df:  main dataframe with all data
    :plots: A line chart shows the trends of urban population growth from 2015 to 2019 in various countries
    '''
    trends_df = main_df[['Indicator Name', 'Country Name', 2015.0, 2016.0, 2017.0, 2018.0, 2019.0]].where(
        main_df['Country Name'].isin(['Switzerland', 'Bahrain', 'Chile', 'Denmark']) & main_df['Indicator Name'].isin(
            ['Urban population growth (annual %)'])).dropna()

    trends_df.plot(y=[2015.0, 2016.0, 2017.0, 2018.0, 2019.0], x='Country Name', kind='line', figsize=(20, 10))


if __name__ == "__main__":

    print('::::Starting Climate Indicators Analysis::::')
    input_file = sys.argv[1]

    df_csv= pd.read_csv(input_file)
    main_df = preprocess_df(df_csv)

    df_years, df_countries = extract_year_country(main_df)
    analyse_population(main_df)
    analyse_agriculture_land(main_df)
    analyse_co2(main_df)
    analyse_trends(main_df)







