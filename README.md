# DbAssignment_10 - Spatial data

## Before all

When you try to download the csv files from http://wfs-kbhkort.kk.dk, remember to edit the download link. Because with the original link, we take exposed areas(f_udsatte_byomraader) as example:
```
http://wfs-kbhkort.kk.dk/k101/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=k101:f_udsatte_byomraader&maxFeatures=50&outputFormat=csv
```
you are only able to get the max 50 features from dataset. If you remove maxFeatures=50 from the link and redownload, then you will get full dataset. Notice some datasets are not necessary, because they do not have 50 features :)
```
http://wfs-kbhkort.kk.dk/k101/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=k101:f_udsatte_byomraader&outputFormat=csv
```

## Exercise 1

Copenhagen has a strategy for improving the living conditions in socially exposed areas (udsatte områder). It a policy which both addresses the social aspects for the people living there, and the physical conditions.

1. How many parks are located in exposed areas?
```MySQL
select count(*) from parkregister, f_udsatte_byomraader
where st_within(parkregister.wkb_geometry, f_udsatte_byomraader.wkb_geometry);
```
```
Output: 8
```

2. How many trees are located in exposed areas?
```MySQL
select count(*) from gadetraer, f_udsatte_byomraader
where st_within(gadetraer.wkb_geometry, f_udsatte_byomraader.wkb_geometry);
```
```
Output: 8
```

## Exercise 2

Copenhagen prides itself for its lively bike culture. There are a number of different data on bike and bike services. But in particular, bikes do not mix well with trucks.

Q. How many bike racks are places along routes for heavy traffic?
```MySQL
select count(*) from cykelstativ, tungvognsnet, vejflade
where cykelstativ.vejkode = vejflade.vejkode and vejflade.vejid = tungvognsnet.vejid;
```
```
Output: 0
```
