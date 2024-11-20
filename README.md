# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session
/kaggle/input/covid-19-india/covid_vaccine_statewise.csv
/kaggle/input/covid-19-india/covid_19_india.csv
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
from plotly.subplots import make_subplots
import datetime
covid_df=pd.read_csv("../input/covid-19-india/covid_19_india.csv")
covid_df.head(10)
Sno	Date	Time	State/UnionTerritory	ConfirmedIndianNational	ConfirmedForeignNational	Cured	Deaths	Confirmed
0	1	2020-01-30	6:00 PM	Kerala	1	0	0	0	1
1	2	2020-01-31	6:00 PM	Kerala	1	0	0	0	1
2	3	2020-02-01	6:00 PM	Kerala	2	0	0	0	2
3	4	2020-02-02	6:00 PM	Kerala	3	0	0	0	3
4	5	2020-02-03	6:00 PM	Kerala	3	0	0	0	3
5	6	2020-02-04	6:00 PM	Kerala	3	0	0	0	3
6	7	2020-02-05	6:00 PM	Kerala	3	0	0	0	3
7	8	2020-02-06	6:00 PM	Kerala	3	0	0	0	3
8	9	2020-02-07	6:00 PM	Kerala	3	0	0	0	3
9	10	2020-02-08	6:00 PM	Kerala	3	0	0	0	3
covid_df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 18110 entries, 0 to 18109
Data columns (total 9 columns):
 #   Column                    Non-Null Count  Dtype 
---  ------                    --------------  ----- 
 0   Sno                       18110 non-null  int64 
 1   Date                      18110 non-null  object
 2   Time                      18110 non-null  object
 3   State/UnionTerritory      18110 non-null  object
 4   ConfirmedIndianNational   18110 non-null  object
 5   ConfirmedForeignNational  18110 non-null  object
 6   Cured                     18110 non-null  int64 
 7   Deaths                    18110 non-null  int64 
 8   Confirmed                 18110 non-null  int64 
dtypes: int64(4), object(5)
memory usage: 1.2+ MB
covid_df.describe()
Sno	Cured	Deaths	Confirmed
count	18110.000000	1.811000e+04	18110.000000	1.811000e+04
mean	9055.500000	2.786375e+05	4052.402264	3.010314e+05
std	5228.051023	6.148909e+05	10919.076411	6.561489e+05
min	1.000000	0.000000e+00	0.000000	0.000000e+00
25%	4528.250000	3.360250e+03	32.000000	4.376750e+03
50%	9055.500000	3.336400e+04	588.000000	3.977350e+04
75%	13582.750000	2.788698e+05	3643.750000	3.001498e+05
max	18110.000000	6.159676e+06	134201.000000	6.363442e+06
vaccine_df=pd.read_csv("../input/covid-19-india/covid_vaccine_statewise.csv")
vaccine_df.head(10)
Updated On	State	Total Doses Administered	Sessions	Sites	First Dose Administered	Second Dose Administered	Male (Doses Administered)	Female (Doses Administered)	Transgender (Doses Administered)	...	18-44 Years (Doses Administered)	45-60 Years (Doses Administered)	60+ Years (Doses Administered)	18-44 Years(Individuals Vaccinated)	45-60 Years(Individuals Vaccinated)	60+ Years(Individuals Vaccinated)	Male(Individuals Vaccinated)	Female(Individuals Vaccinated)	Transgender(Individuals Vaccinated)	Total Individuals Vaccinated
0	16/01/2021	India	48276.0	3455.0	2957.0	48276.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	23757.0	24517.0	2.0	48276.0
1	17/01/2021	India	58604.0	8532.0	4954.0	58604.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	27348.0	31252.0	4.0	58604.0
2	18/01/2021	India	99449.0	13611.0	6583.0	99449.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	41361.0	58083.0	5.0	99449.0
3	19/01/2021	India	195525.0	17855.0	7951.0	195525.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	81901.0	113613.0	11.0	195525.0
4	20/01/2021	India	251280.0	25472.0	10504.0	251280.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	98111.0	153145.0	24.0	251280.0
5	21/01/2021	India	365965.0	32226.0	12600.0	365965.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	132784.0	233143.0	38.0	365965.0
6	22/01/2021	India	549381.0	36988.0	14115.0	549381.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	193899.0	355402.0	80.0	549381.0
7	23/01/2021	India	759008.0	43076.0	15605.0	759008.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	267856.0	491049.0	103.0	759008.0
8	24/01/2021	India	835058.0	49851.0	18111.0	835058.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	296283.0	538647.0	128.0	835058.0
9	25/01/2021	India	1277104.0	55151.0	19682.0	1277104.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	444137.0	832766.0	201.0	1277104.0
10 rows × 24 columns

vaccine_df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7845 entries, 0 to 7844
Data columns (total 24 columns):
 #   Column                               Non-Null Count  Dtype  
---  ------                               --------------  -----  
 0   Updated On                           7845 non-null   object 
 1   State                                7845 non-null   object 
 2   Total Doses Administered             7621 non-null   float64
 3   Sessions                             7621 non-null   float64
 4    Sites                               7621 non-null   float64
 5   First Dose Administered              7621 non-null   float64
 6   Second Dose Administered             7621 non-null   float64
 7   Male (Doses Administered)            7461 non-null   float64
 8   Female (Doses Administered)          7461 non-null   float64
 9   Transgender (Doses Administered)     7461 non-null   float64
 10   Covaxin (Doses Administered)        7621 non-null   float64
 11  CoviShield (Doses Administered)      7621 non-null   float64
 12  Sputnik V (Doses Administered)       2995 non-null   float64
 13  AEFI                                 5438 non-null   float64
 14  18-44 Years (Doses Administered)     1702 non-null   float64
 15  45-60 Years (Doses Administered)     1702 non-null   float64
 16  60+ Years (Doses Administered)       1702 non-null   float64
 17  18-44 Years(Individuals Vaccinated)  3733 non-null   float64
 18  45-60 Years(Individuals Vaccinated)  3734 non-null   float64
 19  60+ Years(Individuals Vaccinated)    3734 non-null   float64
 20  Male(Individuals Vaccinated)         160 non-null    float64
 21  Female(Individuals Vaccinated)       160 non-null    float64
 22  Transgender(Individuals Vaccinated)  160 non-null    float64
 23  Total Individuals Vaccinated         5919 non-null   float64
dtypes: float64(22), object(2)
memory usage: 1.4+ MB
vaccine_df.describe()
Total Doses Administered	Sessions	Sites	First Dose Administered	Second Dose Administered	Male (Doses Administered)	Female (Doses Administered)	Transgender (Doses Administered)	Covaxin (Doses Administered)	CoviShield (Doses Administered)	...	18-44 Years (Doses Administered)	45-60 Years (Doses Administered)	60+ Years (Doses Administered)	18-44 Years(Individuals Vaccinated)	45-60 Years(Individuals Vaccinated)	60+ Years(Individuals Vaccinated)	Male(Individuals Vaccinated)	Female(Individuals Vaccinated)	Transgender(Individuals Vaccinated)	Total Individuals Vaccinated
count	7.621000e+03	7.621000e+03	7621.000000	7.621000e+03	7.621000e+03	7.461000e+03	7.461000e+03	7461.000000	7.621000e+03	7.621000e+03	...	1.702000e+03	1.702000e+03	1.702000e+03	3.733000e+03	3.734000e+03	3.734000e+03	1.600000e+02	1.600000e+02	160.000000	5.919000e+03
mean	9.188171e+06	4.792358e+05	2282.872064	7.414415e+06	1.773755e+06	3.620156e+06	3.168416e+06	1162.978019	1.044669e+06	8.126553e+06	...	8.773958e+06	7.442161e+06	5.641605e+06	1.395895e+06	2.916515e+06	2.627444e+06	4.461687e+07	3.951018e+07	12370.543750	4.547842e+06
std	3.746180e+07	1.911511e+06	7275.973730	2.995209e+07	7.570382e+06	1.737938e+07	1.515310e+07	5931.353995	4.452259e+06	3.298414e+07	...	2.660829e+07	2.225999e+07	1.681650e+07	5.501454e+06	9.567607e+06	8.192225e+06	3.950749e+07	3.417684e+07	12485.026753	1.834182e+07
min	7.000000e+00	0.000000e+00	0.000000	7.000000e+00	0.000000e+00	0.000000e+00	2.000000e+00	0.000000	0.000000e+00	7.000000e+00	...	2.662400e+04	1.681500e+04	9.994000e+03	1.059000e+03	1.136000e+03	5.580000e+02	2.375700e+04	2.451700e+04	2.000000	7.000000e+00
25%	1.356570e+05	6.004000e+03	69.000000	1.166320e+05	1.283100e+04	5.655500e+04	5.210700e+04	8.000000	0.000000e+00	1.331340e+05	...	4.344842e+05	2.326275e+05	1.285605e+05	5.655400e+04	9.248225e+04	5.615975e+04	5.739350e+06	5.023407e+06	1278.750000	7.427550e+04
50%	8.182020e+05	4.547000e+04	597.000000	6.614590e+05	1.388180e+05	3.897850e+05	3.342380e+05	113.000000	1.185100e+04	7.567360e+05	...	3.095970e+06	2.695938e+06	1.805696e+06	2.947270e+05	8.330395e+05	7.887425e+05	3.716590e+07	3.365402e+07	8007.500000	4.022880e+05
75%	6.625243e+06	3.428690e+05	1708.000000	5.387805e+06	1.166434e+06	2.735777e+06	2.561513e+06	800.000000	7.579300e+05	6.007817e+06	...	7.366241e+06	6.969726e+06	5.294763e+06	9.105160e+05	2.499280e+06	2.337874e+06	7.441663e+07	6.685368e+07	19851.000000	3.501562e+06
max	5.132284e+08	3.501031e+07	73933.000000	4.001504e+08	1.130780e+08	2.701636e+08	2.395186e+08	98275.000000	6.236742e+07	4.468251e+08	...	2.243304e+08	1.667575e+08	1.186927e+08	9.224315e+07	9.096888e+07	6.731098e+07	1.349420e+08	1.156684e+08	46462.000000	2.506569e+08
8 rows × 22 columns

covid_df.drop(["Sno","Time","ConfirmedIndianNational","ConfirmedForeignNational"],inplace=True,axis=1)
covid_df.head()
Date	State/UnionTerritory	Cured	Deaths	Confirmed
0	2020-01-30	Kerala	0	0	1
1	2020-01-31	Kerala	0	0	1
2	2020-02-01	Kerala	0	0	2
3	2020-02-02	Kerala	0	0	3
4	2020-02-03	Kerala	0	0	3
covid_df['Date']=pd.to_datetime(covid_df['Date'],format='%Y-%m-%d')
covid_df.head()
Date	State/UnionTerritory	Cured	Deaths	Confirmed
0	2020-01-30	Kerala	0	0	1
1	2020-01-31	Kerala	0	0	1
2	2020-02-01	Kerala	0	0	2
3	2020-02-02	Kerala	0	0	3
4	2020-02-03	Kerala	0	0	3
covid_df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 18110 entries, 0 to 18109
Data columns (total 5 columns):
 #   Column                Non-Null Count  Dtype         
---  ------                --------------  -----         
 0   Date                  18110 non-null  datetime64[ns]
 1   State/UnionTerritory  18110 non-null  object        
 2   Cured                 18110 non-null  int64         
 3   Deaths                18110 non-null  int64         
 4   Confirmed             18110 non-null  int64         
dtypes: datetime64[ns](1), int64(3), object(1)
memory usage: 707.5+ KB
#Active cases
covid_df['Active_cases']=covid_df['Confirmed']-(covid_df['Cured']+covid_df['Deaths'])
covid_df.tail()
Date	State/UnionTerritory	Cured	Deaths	Confirmed	Active_cases
18105	2021-08-11	Telangana	638410	3831	650353	8112
18106	2021-08-11	Tripura	77811	773	80660	2076
18107	2021-08-11	Uttarakhand	334650	7368	342462	444
18108	2021-08-11	Uttar Pradesh	1685492	22775	1708812	545
18109	2021-08-11	West Bengal	1506532	18252	1534999	10215
covid_df['State/UnionTerritory']=covid_df['State/UnionTerritory'].replace('Maharashtra***',"Maharashtra")
covid_df['State/UnionTerritory']=covid_df['State/UnionTerritory'].replace('Bihar****',"Bihar")
covid_df['State/UnionTerritory']=covid_df['State/UnionTerritory'].replace('Madhya Pradesh***',"Madhya Pradesh")
covid_df['State/UnionTerritory']=covid_df['State/UnionTerritory'].replace('Karanataka',"Karnataka")
#pivot table
statewise_data=pd.pivot_table(covid_df, values=['Confirmed','Deaths','Cured'],index="State/UnionTerritory", aggfunc=max)
statewise_data['Recovery Rate']=statewise_data['Cured']*100/statewise_data['Confirmed']
statewise_data['Mortality Rate']=statewise_data['Deaths']*100/statewise_data['Confirmed']
statewise_data=statewise_data.sort_values(by="Confirmed",ascending=False)
statewise_data.style.background_gradient(cmap="CMRmap")
 	Confirmed	Cured	Deaths	Recovery Rate	Mortality Rate
State/UnionTerritory	 	 	 	 	 
Maharashtra	6363442	6159676	134201	96.797865	2.108937
Kerala	3586693	3396184	18004	94.688450	0.501967
Karnataka	2921049	2861499	36848	97.961349	1.261465
Tamil Nadu	2579130	2524400	34367	97.877967	1.332504
Andhra Pradesh	1985182	1952736	13564	98.365591	0.683262
Uttar Pradesh	1708812	1685492	22775	98.635309	1.332797
West Bengal	1534999	1506532	18252	98.145471	1.189056
Delhi	1436852	1411280	25068	98.220276	1.744647
Chhattisgarh	1003356	988189	13544	98.488373	1.349870
Odisha	988997	972710	6565	98.353180	0.663804
Rajasthan	953851	944700	8954	99.040626	0.938721
Gujarat	825085	814802	10077	98.753704	1.221329
Madhya Pradesh	791980	781330	10514	98.655269	1.327559
Haryana	770114	759790	9652	98.659419	1.253321
Bihar	725279	715352	9646	98.631285	1.329971
Telangana	650353	638410	3831	98.163613	0.589065
Punjab	599573	582791	16322	97.201008	2.722271
Assam	576149	559684	5420	97.142232	0.940729
Telengana	443360	362160	2312	81.685312	0.521472
Jharkhand	347440	342102	5130	98.463620	1.476514
Uttarakhand	342462	334650	7368	97.718871	2.151480
Jammu and Kashmir	322771	317081	4392	98.237140	1.360717
Himachal Pradesh	208616	202761	3537	97.193408	1.695460
Himanchal Pradesh	204516	200040	3507	97.811418	1.714780
Goa	172085	167978	3164	97.613389	1.838626
Puducherry	121766	119115	1800	97.822873	1.478245
Manipur	105424	96776	1664	91.796934	1.578388
Tripura	80660	77811	773	96.467890	0.958344
Meghalaya	69769	64157	1185	91.956313	1.698462
Chandigarh	61992	61150	811	98.641760	1.308233
Arunachal Pradesh	50605	47821	248	94.498567	0.490070
Mizoram	46320	33722	171	72.802245	0.369171
Nagaland	28811	26852	585	93.200514	2.030474
Sikkim	28018	25095	356	89.567421	1.270612
Ladakh	20411	20130	207	98.623291	1.014159
Dadra and Nagar Haveli and Daman and Diu	10654	10646	4	99.924911	0.037545
Dadra and Nagar Haveli	10377	10261	4	98.882143	0.038547
Lakshadweep	10263	10165	51	99.045114	0.496931
Cases being reassigned to states	9265	0	0	0.000000	0.000000
Andaman and Nicobar Islands	7548	7412	129	98.198198	1.709062
Unassigned	77	0	0	0.000000	0.000000
Daman & Diu	2	0	0	0.000000	0.000000
# top 10 active cases states
top10ActiveCases=covid_df.groupby(by='State/UnionTerritory').max()[['Active_cases','Date']].sort_values(by=['Active_cases'],ascending=False).reset_index()
fig=plt.figure(figsize=(16,10))
plt.title("Top 10 states with most Active Cases  in India",size=22)
ax=sns.barplot(data=top10ActiveCases.iloc[:10],y='Active_cases',x='State/UnionTerritory',linewidth=2,edgecolor='black')
plt.xlabel("States")
plt.ylabel("Total Active Cases")
plt.show()

# top states with highest number of deaths
top10Deaths=covid_df.groupby(by='State/UnionTerritory').max()[['Deaths','Date']].sort_values(by=['Deaths'],ascending=False).reset_index()
fig=plt.figure(figsize=(16,10))
plt.title("Top 10 states with most Deaths in India",size=22)
ax=sns.barplot(data=top10Deaths.iloc[:10],y='Deaths',x='State/UnionTerritory',linewidth=2,edgecolor='black')
plt.xlabel("States")
plt.ylabel("Total Deaths")
plt.show()

#Growth Trend
fig=plt.figure(figsize=(12,6))
ax=sns.lineplot(data=covid_df[covid_df['State/UnionTerritory'].isin(['Maharashtra','Karnataka','Kerala','Tamil Nadu','Uttar Pradesh'])],x='Date',y='Active_cases',hue='State/UnionTerritory')
#ax.set_title("Top 5 Affected States in India",size=16)

vaccine_df.rename(columns={'Updated On':'Vaccine_Date'},inplace=True)
vaccine_df.head()
Vaccine_Date	State	Total Doses Administered	Sessions	Sites	First Dose Administered	Second Dose Administered	Male (Doses Administered)	Female (Doses Administered)	Transgender (Doses Administered)	...	18-44 Years (Doses Administered)	45-60 Years (Doses Administered)	60+ Years (Doses Administered)	18-44 Years(Individuals Vaccinated)	45-60 Years(Individuals Vaccinated)	60+ Years(Individuals Vaccinated)	Male(Individuals Vaccinated)	Female(Individuals Vaccinated)	Transgender(Individuals Vaccinated)	Total Individuals Vaccinated
0	16/01/2021	India	48276.0	3455.0	2957.0	48276.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	23757.0	24517.0	2.0	48276.0
1	17/01/2021	India	58604.0	8532.0	4954.0	58604.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	27348.0	31252.0	4.0	58604.0
2	18/01/2021	India	99449.0	13611.0	6583.0	99449.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	41361.0	58083.0	5.0	99449.0
3	19/01/2021	India	195525.0	17855.0	7951.0	195525.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	81901.0	113613.0	11.0	195525.0
4	20/01/2021	India	251280.0	25472.0	10504.0	251280.0	0.0	NaN	NaN	NaN	...	NaN	NaN	NaN	NaN	NaN	NaN	98111.0	153145.0	24.0	251280.0
5 rows × 24 columns

vaccine_df.isnull().sum()
Vaccine_Date                              0
State                                     0
Total Doses Administered                224
Sessions                                224
 Sites                                  224
First Dose Administered                 224
Second Dose Administered                224
Male (Doses Administered)               384
Female (Doses Administered)             384
Transgender (Doses Administered)        384
 Covaxin (Doses Administered)           224
CoviShield (Doses Administered)         224
Sputnik V (Doses Administered)         4850
AEFI                                   2407
18-44 Years (Doses Administered)       6143
45-60 Years (Doses Administered)       6143
60+ Years (Doses Administered)         6143
18-44 Years(Individuals Vaccinated)    4112
45-60 Years(Individuals Vaccinated)    4111
60+ Years(Individuals Vaccinated)      4111
Male(Individuals Vaccinated)           7685
Female(Individuals Vaccinated)         7685
Transgender(Individuals Vaccinated)    7685
Total Individuals Vaccinated           1926
dtype: int64
vaccination=vaccine_df.drop(columns=['Sputnik V (Doses Administered)','AEFI','18-44 Years (Doses Administered)','45-60 Years (Doses Administered)','60+ Years (Doses Administered)'],axis=1)
vaccination.head()
Vaccine_Date	State	Total Doses Administered	Sessions	Sites	First Dose Administered	Second Dose Administered	Male (Doses Administered)	Female (Doses Administered)	Transgender (Doses Administered)	Covaxin (Doses Administered)	CoviShield (Doses Administered)	18-44 Years(Individuals Vaccinated)	45-60 Years(Individuals Vaccinated)	60+ Years(Individuals Vaccinated)	Male(Individuals Vaccinated)	Female(Individuals Vaccinated)	Transgender(Individuals Vaccinated)	Total Individuals Vaccinated
0	16/01/2021	India	48276.0	3455.0	2957.0	48276.0	0.0	NaN	NaN	NaN	579.0	47697.0	NaN	NaN	NaN	23757.0	24517.0	2.0	48276.0
1	17/01/2021	India	58604.0	8532.0	4954.0	58604.0	0.0	NaN	NaN	NaN	635.0	57969.0	NaN	NaN	NaN	27348.0	31252.0	4.0	58604.0
2	18/01/2021	India	99449.0	13611.0	6583.0	99449.0	0.0	NaN	NaN	NaN	1299.0	98150.0	NaN	NaN	NaN	41361.0	58083.0	5.0	99449.0
3	19/01/2021	India	195525.0	17855.0	7951.0	195525.0	0.0	NaN	NaN	NaN	3017.0	192508.0	NaN	NaN	NaN	81901.0	113613.0	11.0	195525.0
4	20/01/2021	India	251280.0	25472.0	10504.0	251280.0	0.0	NaN	NaN	NaN	3946.0	247334.0	NaN	NaN	NaN	98111.0	153145.0	24.0	251280.0
#Male vs Female Vaccination

male=vaccination['Male(Individuals Vaccinated)'].sum()
female=vaccination['Female(Individuals Vaccinated)'].sum()
px.pie(names=["Male","Female"],values=[male,female],title="Male vs Female Vaccination")
vaccine=vaccine_df[vaccine_df.State!='India']
vaccine.head()
Vaccine_Date	State	Total Doses Administered	Sessions	Sites	First Dose Administered	Second Dose Administered	Male (Doses Administered)	Female (Doses Administered)	Transgender (Doses Administered)	...	18-44 Years (Doses Administered)	45-60 Years (Doses Administered)	60+ Years (Doses Administered)	18-44 Years(Individuals Vaccinated)	45-60 Years(Individuals Vaccinated)	60+ Years(Individuals Vaccinated)	Male(Individuals Vaccinated)	Female(Individuals Vaccinated)	Transgender(Individuals Vaccinated)	Total Individuals Vaccinated
212	16/01/2021	Andaman and Nicobar Islands	23.0	2.0	2.0	23.0	0.0	12.0	11.0	0.0	...	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	23.0
213	17/01/2021	Andaman and Nicoba

<!---
Pavithra-cmy/Pavithra-cmy is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
