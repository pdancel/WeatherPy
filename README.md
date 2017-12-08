
<dl>
  <dt>Analysis</dt>
  <dd>1. Cities with latitudes closer to the equator are warmer than cities farther away from the equator.</dd>
  <dd>2. A majority of the world's cities experience wind speeds of 20 mph or less. Fewer cities experience wind speeds of 20 mph or more.</dd>
  <dd>3. Latitude does not necessarily have a big impact on the humidity of a city.</dd>
</dl>


```python
# Dependencies
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import requests as req
import time
import openweathermapy.core as owm
import csv
from citipy import citipy
```


```python
cities = pd.read_csv("https://raw.githubusercontent.com/wingchen/citipy/master/citipy/worldcities.csv")
cities.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>City</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ad</td>
      <td>andorra la vella</td>
      <td>42.500000</td>
      <td>1.516667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ad</td>
      <td>canillo</td>
      <td>42.566667</td>
      <td>1.600000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ad</td>
      <td>encamp</td>
      <td>42.533333</td>
      <td>1.583333</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ad</td>
      <td>la massana</td>
      <td>42.550000</td>
      <td>1.516667</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ad</td>
      <td>les escaldes</td>
      <td>42.500000</td>
      <td>1.533333</td>
    </tr>
  </tbody>
</table>
</div>




```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
   # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
print(len(cities))
print(cities)
```

    617
    ['busselton', 'mataura', 'jiddah', 'sandpoint', 'cape town', 'masaurhi', 'gold coast', 'ushuaia', 'port macquarie', 'hobart', 'airai', 'saskylakh', 'luba', 'deep river', 'khatanga', 'nova olimpia', 'nikolskoye', 'xining', 'teacapan', 'korla', 'riyadh', 'biltine', 'usinsk', 'kenai', 'yerofey pavlovich', 'zhigansk', 'bluff', 'lebu', 'bethel', 'vanimo', 'kavieng', 'carnarvon', 'bengkulu', 'geraldton', 'rikitea', 'viedma', 'hithadhoo', 'barrow', 'esperance', 'touros', 'barentsburg', 'sentyabrskiy', 'thompson', 'lovozero', 'jamestown', 'derzhavinsk', 'pingliang', 'vieques', 'mogadishu', 'albany', 'gubkinskiy', 'nizhneyansk', 'atuona', 'ewa beach', 'karratha', 'kahului', 'neiafu', 'port alfred', 'keroka', 'abbeville', 'taitung', 'sibolga', 'saldanha', 'guerrero negro', 'morgan city', 'kapaa', 'dikson', 'masvingo', 'yulara', 'inirida', 'taolanaro', 'nioro', 'santiago del estero', 'fort saint john', 'adrar', 'talawdi', 'avarua', 'sunnyvale', 'nome', 'kerema', 'punta arenas', 'luena', 'dezhou', 'talnakh', 'ilulissat', 'codrington', 'salalah', 'fare', 'hilo', 'sambava', 'turayf', 'chuy', 'tasiilaq', 'qaanaaq', 'lang son', 'ponta do sol', 'haibowan', 'tuatapere', 'taltal', 'fortuna', 'buchanan', 'comodoro rivadavia', 'cherskiy', 'souillac', 'vaini', 'georgetown', 'saleaula', 'olonets', 'lavrentiya', 'maniitsoq', 'vila franca do campo', 'lolua', 'new norfolk', 'paramonga', 'hermanus', 'alofi', 'karpathos', 'kununurra', 'sorland', 'college station', 'tungawan', 'mrirt', 'castro', 'dano', 'hurricane', 'saint-philippe', 'raudeberg', 'kamariotissa', 'basco', 'arlit', 'husavik', 'yellowknife', 'bredasdorp', 'saint-joseph', 'ostrovnoy', 'puerto ayora', 'ilhabela', 'tuktoyaktuk', 'gorontalo', 'butaritari', 'ranfurly', 'illoqqortoormiut', 'kesinga', 'mount gambier', 'aklavik', 'bambous virieux', 'kruisfontein', 'ahipara', 'klaksvik', 'hamilton', 'stillwater', 'east london', 'san cristobal', 'tessalit', 'kalmunai', 'buturlino', 'attawapiskat', 'kavaratti', 'sitka', 'senador jose porfirio', 'gazanjyk', 'ribeira grande', 'dumai', 'disna', 'ancud', 'ojinaga', 'trelew', 'harnosand', 'naze', 'tiarei', 'jalu', 'guiratinga', 'provideniya', 'port elizabeth', 'doka', 'port lincoln', 'lompoc', 'tiksi', 'port hardy', 'townsville', 'natal', 'ancona', 'bismarck', 'mar del plata', 'igarka', 'chapleau', 'berlevag', 'dalby', 'lyubytino', 'rawson', 'diego de almagro', 'garbolovo', 'hobyo', 'kiama', 'coari', 'qaqortoq', 'arraial do cabo', 'ilebo', 'torbay', 'solok', 'coruripe', 'balud', 'harer', 'colon', 'faanui', 'cravo norte', 'ust-kamchatsk', 'severo-kurilsk', 'bronnoysund', 'deputatskiy', 'kendari', 'muscat', 'kosh-agach', 'nakonde', 'shangzhi', 'vaitupu', 'los llanos de aridane', 'iqaluit', 'verkhoyansk', 'victoria', 'chokurdakh', 'todos santos', 'palmer', 'cabo san lucas', 'hasaki', 'norman wells', 'tutoia', 'pangnirtung', 'atar', 'san andres', 'north platte', 'palabuhanratu', 'kunnamkulam', 'port hedland', 'longyearbyen', 'ahuimanu', 'amparafaravola', 'birjand', 'nhulunbuy', 'luderitz', 'nador', 'suffolk', 'nelson bay', 'kodiak', 'zolotinka', 'burica', 'lahij', 'willowmore', 'pag', 'blythe', 'orangeburg', 'antofagasta', 'manggar', 'ye', 'mahebourg', 'ariquemes', 'lebedinyy', 'general roca', 'morros', 'narsaq', 'newport', 'pevek', 'atikokan', 'kaitangata', 'coaldale', 'nanortalik', 'andilamena', 'elko', 'havoysund', 'aloleng', 'mount isa', 'tubruq', 'laguna', 'dudinka', 'kirkwall', 'dwarka', 'muyezerskiy', 'hokitika', 'balakhninskiy', 'wawa', 'prado', 'puerto escondido', 'aripuana', 'numan', 'pandan', 'crestview', 'chicama', 'chubbuck', 'cockburn town', 'avera', 'lodeynoye pole', 'tecoanapa', 'lima', 'san patricio', 'bac lieu', 'trat', 'oconomowoc', 'mys shmidta', 'peniche', 'vilyuysk', 'upernavik', 'tshikapa', 'atasu', 'jacareacanga', 'muisne', 'mehamn', 'letterkenny', 'sao gabriel da cachoeira', 'hlotse', 'marrakesh', 'indiana', 'owando', 'westport', 'sao filipe', 'leningradskiy', 'clyde river', 'saint augustine', 'djibo', 'belushya guba', 'manoel urbano', 'northam', 'marsa matruh', 'salinas', 'cap malheureux', 'bathsheba', 'gat', 'pak phanang', 'muros', 'natchitoches', 'henties bay', 'maykain', 'kauhajoki', 'dzhusaly', 'altay', 'tombouctou', 'komsomolskiy', 'likasi', 'roma', 'tsihombe', 'saint george', 'malwan', 'tukrah', 'grindavik', 'marzuq', 'gemena', 'sretensk', 'turbat', 'weihai', 'victor harbor', 'yenagoa', 'bonnyville', 'himora', 'mocambique', 'anjozorobe', 'dawei', 'galiwinku', 'lasa', 'cidreira', 'itigi', 'miri', 'petauke', 'poum', 'faya', 'beringovskiy', 'teya', 'rehoboth', 'havelock', 'santa cruz', 'saint-augustin', 'kodinsk', 'hailar', 'presidencia roque saenz pena', 'clarence town', 'astana', 'krasnoselkup', 'ipilan', 'warri', 'kutum', 'diamantino', 'jumla', 'barawe', 'bom jesus', 'taoudenni', 'vung tau', 'krasnorechenskiy', 'troy', 'tura', 'wolmaranstad', 'vestmannaeyjar', 'samarai', 'asau', 'umba', 'oswego', 'launceston', 'dom pedro', 'pryluky', 'yaroslavskaya', 'rocha', 'rundu', 'minas', 'umm lajj', 'rock sound', 'marsassoum', 'angoram', 'doctor pedro p. pena', 'broome', 'nauchnyy gorodok', 'presidente bernardes', 'vicuna', 'dali', 'alotau', 'kaka', 'uyar', 'kouango', 'highland park', 'hauterive', 'coihaique', 'bontang', 'vestmanna', 'tigil', 'armacao de pera', 'lebanon', 'camargo', 'bagula', 'levokumka', 'saint-pierre', 'hirara', 'ishigaki', 'chapais', 'guaymas', 'bonavista', 'constitucion', 'ambon', 'hay river', 'rio gallegos', 'ushtobe', 'yumen', 'irondequoit', 'matamoros', 'kandi', 'zanjan', 'mortka', 'taksimo', 'penzance', 'santa maria', 'fillan', 'kloulklubed', 'loukhi', 'qaracala', 'zhezkazgan', 'albanel', 'chumikan', 'jinxiang', 'ouesso', 'bolungarvik', 'nago', 'grand gaube', 'morristown', 'itarema', 'bundaberg', 'halifax', 'magdalena', 'namibe', 'bardiyah', 'tsiroanomandidy', 'gurgan', 'necochea', 'linjiang', 'praia da vitoria', 'paita', 'bereznik', 'batouri', 'tadine', 'bayonet point', 'venado tuerto', 'solnechnyy', 'khandbari', 'antsohihy', 'mahibadhoo', 'poopo', 'samusu', 'eureka', 'diffa', 'amahai', 'sioux lookout', 'vila velha', 'marabba', 'puerto narino', 'romanovo', 'port blair', 'half moon bay', 'grand centre', 'hami', 'plouzane', 'tchollire', 'cabedelo', 'chegdomyn', 'sola', 'nisia floresta', 'skvyra', 'ararat', 'malmyzh', 'yar-sale', 'rypefjord', 'santa vitoria do palmar', 'mukhen', 'osmena', 'the valley', 'fomboni', 'dunedin', 'santa rosa', 'graaff-reinet', 'pangody', 'kanniyakumari', 'boyolangu', 'amapa', 'tianpeng', 'toliary', 'cayenne', 'kuala terengganu', 'casper', 'broken hill', 'hambantota', 'starobaltachevo', 'ontario', 'brandon', 'banjar', 'vamvakofiton', 'suruc', 'scarborough', 'abha', 'narasannapeta', 'abu dhabi', 'vanavara', 'fairbanks', 'berea', 'meyungs', 'darhan', 'skibbereen', 'uusikaupunki', 'isangel', 'sur', 'kaeo', 'svetlyy', 'doctor arroyo', 'high level', 'nishihara', 'iskateley', 'buraydah', 'marovoay', 'mweka', 'minsk mazowiecki', 'pervomayskaya', 'price', 'walvis bay', 'verkh-usugli', 'kirkuk', 'san clemente', 'jabiru', 'areosa', 'bandarbeyla', 'shadrinsk', 'rungata', 'nouadhibou', 'guaruja', 'veraval', 'bartica', 'alta floresta', 'kudahuvadhoo', 'george', 'anchorage', 'kuche', 'yunjinghong', 'ormara', 'ponta delgada', 'honningsvag', 'menongue', 'wanxian', 'soyo', 'noumea', 'semporna', 'tres passos', 'sabla', 'khandyga', 'bubaque', 'ranong', 'hunza', 'farafangana', 'moussoro', 'poso', 'aksu', 'ipora', 'marcona', 'sayyan', 'skalistyy', 'flinders', 'san bartolome de tirajana', 'paka', 'las vegas', 'krasnokamensk', 'yanam', 'ornskoldsvik', 'lander', 'tidore', 'buala', 'okhotsk', 'shenjiamen', 'amderma', 'porto santo', 'las cruces', 'oranjestad', 'akdepe', 'benguela', 'marshall', 'nacogdoches', 'gillette', 'honolulu']



```python
cities_df = pd.DataFrame({'City': cities})
cities_df['Latitude']=""
cities_df['Temperature']=""
cities_df['Humidity']=""
cities_df['Cloudiness']=""
cities_df['Wind Speed']=""
cities_df.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Latitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>busselton</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>mataura</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>jiddah</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>sandpoint</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>cape town</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
api_key = "af43b87057912e9dc99d8f938dcb4a7e"
url = "http://api.openweathermap.org/data/2.5/weather?"
settings = {"units": "imperial", "appid": api_key}
```


```python
city_list = []
# temp = weather["main"]["temp"]
# lat = weather["coord"]['lat']
# hum = weather["main"]["humidity"]
# cloud = weather["clouds"]['all']
# wind_speed = weather["wind"]['speed']
for index, row in cities_df.iterrows():
    try:
        weather = owm.get_current(row['City'], **settings)
        cities_df.set_value(index, 'Latitude', weather["coord"]['lat'])
        cities_df.set_value(index, "Temperature", weather["main"]["temp"])
        cities_df.set_value(index, "Humidity", weather["main"]["humidity"] )
        cities_df.set_value(index, "Cloudiness", weather["clouds"]["all"])
        cities_df.set_value(index, "Wind Speed",  weather["wind"]["speed"])
        city_list.append(row['City'])
    except:
         print(row['City'])
        
       
```

    mataura
    jiddah
    masaurhi
    airai
    nikolskoye
    barentsburg
    sentyabrskiy
    nizhneyansk
    taolanaro
    fort saint john
    talawdi
    haibowan
    saleaula
    lolua
    mrirt
    raudeberg
    illoqqortoormiut
    attawapiskat
    disna
    jalu
    ust-kamchatsk
    vaitupu
    palabuhanratu
    zolotinka
    burica
    ye
    kaitangata
    tubruq
    avera
    bac lieu
    mys shmidta
    hlotse
    belushya guba
    gat
    henties bay
    maykain
    dzhusaly
    tombouctou
    tsihombe
    malwan
    tukrah
    marzuq
    himora
    mocambique
    galiwinku
    faya
    krasnoselkup
    barawe
    wolmaranstad
    doctor pedro p. pena
    alotau
    ushtobe
    zhezkazgan
    bolungarvik
    itarema
    gurgan
    samusu
    grand centre
    amapa
    toliary
    meyungs
    jabiru
    rungata
    kuche
    yunjinghong
    soyo
    sabla
    hunza
    marcona
    skalistyy
    san bartolome de tirajana
    tidore
    amderma



```python
cities_df = cities_df[cities_df['City'].isin(city_list)]
cities_df.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Latitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>busselton</td>
      <td>-33.65</td>
      <td>75.45</td>
      <td>76</td>
      <td>44</td>
      <td>9.98</td>
    </tr>
    <tr>
      <th>3</th>
      <td>sandpoint</td>
      <td>48.28</td>
      <td>28.4</td>
      <td>79</td>
      <td>1</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cape town</td>
      <td>-33.93</td>
      <td>68</td>
      <td>72</td>
      <td>0</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>6</th>
      <td>gold coast</td>
      <td>-28</td>
      <td>80.6</td>
      <td>74</td>
      <td>0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ushuaia</td>
      <td>-54.8</td>
      <td>42.8</td>
      <td>75</td>
      <td>40</td>
      <td>20.8</td>
    </tr>
  </tbody>
</table>
</div>




```python
cities_df.to_csv('WeatherPyData.csv') 
```


```python
#Temperature (F) vs. Latitude
fig = plt.figure(figsize=(10,10))
ax1 = fig.add_subplot(1,1,1)

ax1.scatter(cities_df['Latitude'], cities_df['Temperature'], c='lightcoral', marker='o',edgecolors="black", alpha=0.75)

plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.title('City Temperature vs. Latitude')
plt.ylim(-100, 150)
plt.xlim(-90, 90)

plt.grid()

plt.savefig('temp_lat.png')
plt.show()

```


![png](output_9_0.png)



```python
#Humidity (%) vs. Latitude
fig = plt.figure(figsize=(10,10))
ax1 = fig.add_subplot(1,1,1)

ax1.scatter(cities_df['Latitude'], cities_df['Humidity'], c='lightskyblue', marker='o',edgecolors="black", alpha=0.75)

plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.title('City Humidity vs. Latitude')
plt.ylim(-20, 120)
plt.xlim(-90, 90)

plt.grid()

plt.savefig('hum_lat.png')
plt.show()
```


![png](output_10_0.png)



```python
#Cloudiness (%) vs. Latitude
fig = plt.figure(figsize=(10,10))
ax1 = fig.add_subplot(1,1,1)

ax1.scatter(cities_df['Latitude'], cities_df['Cloudiness'], c='Gold', marker='o',edgecolors="black", alpha=0.75)

plt.xlabel("Latitude")
plt.ylabel("Cloudiness(%)")
plt.title('City Cloudiness vs. Latitude')
plt.ylim(-20, 120)
plt.xlim(-90, 90)

plt.grid()

plt.savefig('cloud_lat.png')
plt.show()
```


![png](output_11_0.png)



```python
#Wind Speed (%) vs. Latitude
fig = plt.figure(figsize=(10,10))
ax1 = fig.add_subplot(1,1,1)

ax1.scatter(cities_df['Latitude'], cities_df['Wind Speed'], c='Gold', marker='o',edgecolors="black", alpha=0.75)

plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
plt.title('City Wind Speed vs. Latitude')
plt.ylim(-5, 50)
plt.xlim(-90, 90)

plt.grid()

plt.savefig('wind_lat.png')
plt.show()
```


![png](output_12_0.png)

