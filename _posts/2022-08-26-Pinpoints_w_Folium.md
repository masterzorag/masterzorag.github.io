---
layout: post
title:  "Pinpoints w/ Folium"
date:   2022-08-26 00:00:00 +0200
categories: python3 sample
---
# Pinpoints on a Map with [Folium](https://python-visualization.github.io/folium)

assuming an IP list with geolocatiion info like following:
```
sqlite3 $HOME/Downloads/banlist/banlist_v2.db "SELECT ip,latitude,longitude,city FROM blacklist" > list.ips
#['112.85.42.227', '31.299', '120.585', 'Suzhou']
...
```

build a map adding Folium Markers iterating single line:
```
import folium

m = folium.Map(location=[46.0117, 10.93542], zoom_start=2.6)

input_file = f"list.ips"

with open(input_file, encoding="utf-8") as file_obj:
    line = [line.strip() for line in file_obj.readlines()]
    for address in line:
        data = address.split("|")
        folium.Marker([data[1], data[2]], popup=data[0], tooltip=data[3]+", "+data[0]).add_to(m)
#       folium.Marker([45.3288, -121.6625], popup=address, tooltip=address).add_to(m)

m.save("map_sample.html")

```
[map sample](https://masterzorag.github.io/map_sample.html) from gh-pages is using leflet.js and OpenStreetMap
