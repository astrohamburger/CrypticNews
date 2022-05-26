# CrypticNews

## Finding News Tone Goldstein from GDELT data in MongoDB

This is a set of aggregations working on GDELT 2.0 (https:/gdeltproject.org) data set in MongoDB. 

## Quick Start

Load the GDELT data into MongoDB as described in https://github.com/jdrumgoole/gdelttools.
Load the historic Bitcoin price hourly from https://www.cryptodatadownload.com/cdd/Gemini_BTCUSD_1h.csv in a new Bitcoin_hour collection

Run the first aggregation allEvents on the eventscsv collection

  allEvents:  
      Input       ->  eventscsv
      Aggregation ->  group events hourly by YYYYMMDDH24
                      new fields  eventsGoldstein_avg
                                  eventsGoldstein_min
                                  eventsGoldstein_max
                                  eventsGoldstein_gamma
                                  eventsTone_avg
                                  eventsTone_min
                                  eventsTone_max
                                  eventsTone_gamma
      Output      ->  allEvents

  cryptoEvents:  
      Input       ->  eventscsv
      Aggregation ->  group events hourly by YYYYMMDDH24
                      new fields  cryptoGoldstein_avg
                                  cryptoGoldstein_min
                                  cryptoGoldstein_max
                                  cryptoGoldstein_gamma
                                  cryptoTone_avg
                                  cryptoTone_min
                                  cryptoTone_max
                                  cryptoTone_gamma
      Output      ->  cryptoEvents
	  
  priceEvents:  
      Input       ->  Bitcoin_hour
      Aggregation ->  filter the date
					  new fields  DayDate '%Y%m%d%H'

      Output      ->  priceEvents	  
	  
  allPriceEvents:  
      Input       ->  allEvents
      Aggregation ->  lookup on cryptoEvents
					  lookup on priceEvents
                      unwind the array
      Output      ->  allPriceEvents	  
 
	  
// TODO
