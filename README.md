# Sachin-vs-Virat-Analysis
import warnings
warnings.filterwarnings('ignore')
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
df = pd.read_csv('ODI_data.csv')
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.expand_frame_repr', False)
pd.set_option('max_colwidth', -1)

df = df[df['Innings Runs Scored Num'] != '-']
df = df.dropna(subset = ['Innings Runs Scored Num'])
df['Innings Date'] = pd.to_datetime(df['Innings Date'])
df['year'] = df['Innings Date'].dt.year

df['Innings Runs Scored Num'] = df['Innings Runs Scored Num'].astype('int')
df['Innings Balls Faced'] = df['Innings Balls Faced'].astype('int') 
df["100"] = df["100"].astype('int') 
df["50"] = df["50"].astype('int') 

sachin = df[(df.year >= 1994) & (df.year <= 2003)]
virat = df[(df.year >= 2009) & (df.year <= 2018)]

sdf = sachin[sachin['Innings Player'] == 'SR Tendulkar']
vdf = virat[virat['Innings Player'] == 'V Kohli']

print('RUNS Per INNINGS')
sRPI = sum(sdf['Innings Runs Scored Num'])/len(sdf)
vRPI = sum(vdf['Innings Runs Scored Num'])/len(vdf)
print("   Sachin Tendulkar's RPI: ",round(sRPI,3))
print("   Virat Kohli's RPI: ",round(vRPI,3))
print('STRIKE RATE')
sSR = sum(sdf['Innings Runs Scored Num'])/sum(sdf['Innings Balls Faced']) * 100
vSR = sum(vdf['Innings Runs Scored Num'])/sum(vdf['Innings Balls Faced']) * 100
print("   Sachin Tendulkar's Strike Rate: ",round(sSR,3))
print("   Virat Kohli's Strike Rate: ",round(vSR,3))
print('100s SCORED')
s100 = sum(sdf["100"])
v100 = sum(vdf["100"])
print("   Sachin Tendulkar's 100s: ",s100)
print("   Virat Kohli's 100s: ",v100)
print('50s SCORED')
s50 = sum(sdf["50"])
v50 = sum(vdf["50"])
print("   Sachin Tendulkar's 50s: ",s50)
print("   Virat Kohli's 50s: ",v50)
print('TEAM CONTRIBUTION')
sTC = 100 * (sum(sdf['Innings Runs Scored Num'])/sum(sachin[sachin.Country == 'India']['Innings Runs Scored Num']))
vTC = 100 * (sum(sdf['Innings Runs Scored Num'])/sum(virat[virat.Country == 'India']['Innings Runs Scored Num']))
print("   Sachin Tendulkar's Team Contribution: ",round(sTC,3))
print("   Virat Kohli's Team Contribution: ",round(vTC,3))

#comparing the two players with others players form the specified timeline
print('Top 10 Run Scorers from 1994 to 2003')
print(sachin.groupby(['Innings Player'])['Innings Runs Scored Num'].sum().sort_values(ascending = False).head(10))
print(' ')
print('Top 10 Run Scorers from 2009 to 2018')
print(virat.groupby(['Innings Player'])['Innings Runs Scored Num'].sum().sort_values(ascending = False).head(10))

nsachin = sachin[sachin['Innings Player'] != 'SR Tendulkar']
nvirat = virat[virat['Innings Player'] != 'V Kohli']

# Normalized RPI
print('NORMALIZED RPI')
print("   Sachin Tendulkar's Normalized RPI: ",round((sum(sachin['Innings Runs Scored Num'])/len('sachin'))/(sum(nsachin['Innings Runs Scored Num'])/len('nsachin')),3))
print("   Virat Kohli's Normalized RPI: ",round((sum(virat['Innings Runs Scored Num'])/len('virat'))/(sum(nvirat['Innings Runs Scored Num'])/len('nvirat')),3))
# Normalized Strike rate
print('NORMALIZED STRIKE RATE')
print("   Sachin Tendulkar's Normalized Strike Rate: ",round(100*(sum(sachin['Innings Runs Scored Num'])/sum(sachin['Innings Balls Faced']))/(sum(nsachin['Innings Runs Scored Num'])/sum(nsachin['Innings Balls Faced'])),3))
print("   Virat Kohli's Normalized Strike Rate: ",round(100*(sum(virat['Innings Runs Scored Num'])/sum(virat['Innings Balls Faced']))/(sum(nvirat['Innings Runs Scored Num'])/sum(nvirat['Innings Balls Faced'])),3))
# Normalized 100's
#  Total number of matches plated by Sachin/Virat 
# ------------------------------------------------
# Total number of centuries scored by Sachin/Virat
print('NORMALIZED 100s SCORED')
print("   Sachin Tendulkar's Normalized 100s: ",round(len(sdf)/sum(sdf['100']),3))
print("   Virat Kohli's Normalized 100s: ",round(len(vdf)/sum(vdf['100']),3))
# Normalized 50's
print('NORMALIZED 50s SCORED')
print("   Sachin Tendulkar's Normalized 50s: ",round(len(sdf)/sum(sdf['50']),3))
print("   Virat Kohli's Normalized 50s: ",round(len(vdf)/sum(vdf['50']),3))
# Team Contribution
#     Team Contribution of Sachin/Virat
# -----------------------------------------
# Team Contribution of other Indian Batsmen
print('NORMALIZED TEAM CONTRIBUTION')
nsdf = nsachin[nsachin['Country'] == 'India']
nvdf = nvirat[nvirat['Country'] == 'India']
sTC = 100 * sum(sdf['Innings Runs Scored Num'])/sum(sachin[sachin.Country == 'India']['Innings Runs Scored Num'])
osTC = 100 * sum(nsdf['Innings Runs Scored Num'])/sum(nsachin[nsachin.Country == 'India']['Innings Runs Scored Num'])
print("   Sachin Tendulkar's Normalized Team Contribution: ",round(sTC/osTC,3))
vTC = 100 * sum(vdf['Innings Runs Scored Num'])/sum(virat[virat.Country == 'India']['Innings Runs Scored Num'])
ovTC = 100 * sum(nvdf['Innings Runs Scored Num'])/sum(nvirat[nvirat.Country == 'India']['Innings Runs Scored Num'])
print("   Virat Kohli's Normalized Team Contribution: ",round(vTC/ovTC,3))
