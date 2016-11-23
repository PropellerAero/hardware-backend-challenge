# AeroPoint programming test

##Background

Centimetre grade GNSS<sup>[1]</sup> positions are generally obtained using differential carrier-phase techniques. These methods use a pair of GNSS receivers, one called the “base” which stays at a fixed, known location and the other known as the “rover” which is placed at unknown locations which are to be measured.

By combining satellite signal and carrier wave observation data from both receivers, variations in the satellite orbit and atmospheric conditions which lead to error in the position solution can be cancelled out.

Networks of fixed GNSS base stations known as continuously operating reference station (CORS) networks are operated by government departments and private companies. These networks broadcast the base station observation data as well as their well-known position in real time over the internet for use in surveying and machine control applications, most also make archives of the data available for post-processing.

We use this archived data to process the observation data from our AeroPoints into accurate ground control points<sup>[2]</sup> on our servers. By doing this the AeroPoint receivers do not need a persistent internet connection, simplifying the hardware.

There is a standardised open-source ASCII file format called RINEX (Receiver Independent Exchange Format) which these observations are made available in.

One of these networks in the NOAA CORS network in the United States. RINEX observation data from each of their base stations is published in 1 hour blocks on their FTP server at ftp://www.ngs.noaa.gov/cors/rinex. You can read more about the network here: http://geodesy.noaa.gov/CORS/


##The Task

Write a command line tool in a language of your choice that given a base station ID, start time and end time downloads all the 1 hour blocks that span the time period given and merges them into a single RINEX observation file called `<base_station_id>.obs`. The start and end times will be given as ISO8601 strings.

The RINEX observation files are stored in gzip’d archives on the server at paths similar to this example:

ftp://www.ngs.noaa.gov/cors/rinex/2016/327/nybp/nybp327b.16o.gz

Where:
- `2016` is the year
- `327` is the day of the year
- `nybp` is the base station ID
- `b` refers to the hour block. `a` refers to the hour from 12am-1am (GPS time), `x` refers to 11pm-12am, and so on
- The `“16”` in the `.16o` extension is the year again. Files from 2017 will end with `.17o`

####Example

Calling the tool like so:

`./grab_data nybp 2016-11-22T23:11:22Z 2016-11-23T01:33:44Z`

Should download the following files from the server and generate the example output `example.obs` contained in this repository:

    ftp://www.ngs.noaa.gov/cors/rinex/2016/327/nybp/nybp327x.16o.gz
    ftp://www.ngs.noaa.gov/cors/rinex/2016/328/nybp/nybp328a.16o.gz
    ftp://www.ngs.noaa.gov/cors/rinex/2016/328/nybp/nybp328b.16o.gz

Note: only a few days worth of the hourly logs are kept on the server, feel free to use a later time period if the example files disappear from the server. If you feel like a challenge, handle the case where the full day logs (e.g. `nybp3270.16o.gz`) are used if the hourly log has been removed from the server.

###Things we’ll be looking for:
- Ease of installation and use
- Elegant error handling
- Appropriately commented code
- Thorough testing methodology

You should include a short README file that documents the steps to install and run the tool and any design trade-offs you have made. Your command-line utility should run on some *nix, preferably Mac OS. Otherwise, let us know what you ran it on.

Please send us a link to a git repository containing your solution when you're done.

###Helpful resources:
- The [TEQC toolkit](https://www.unavco.org/software/data-processing/teqc/teqc.html) is a CLI tool that can merge RINEX files 
- There is a useful GPS calendar [here](http://navigationservices.agi.com/GNSSWeb/)

Feel free to use any other libraries or toolkits you need to complete the task. Please include attributions to any libraries you use in your README file.

Please don’t hesitate to ask us for clarification. The task should take a few hours, if you find it is taking significantly longer, please let us know!

##Glossary
[1] GNSS - The catch-all term for satellite based global navigation systems like GPS, GLONASS and BeiDou

[2] Ground control points -  Visible markers placed on the terrain of an aerial survey with known coordinates used to improve the accuracy of the generated model