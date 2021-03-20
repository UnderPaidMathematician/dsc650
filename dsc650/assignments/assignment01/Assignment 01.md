---
title: Assignment 1
subtitle: Computer performance, reliability, and scalability calculation
author: Jason Ismail
---

## 1.2 

#### a. Data Sizes
# (Notes shown below)

| Data Item                                  | Size per Item | 
|--------------------------------------------|--------------:|
| 128 character message.                     | 512 Bytes assuming UCS-4 encoding (largest encoding)|
| 1024x768 PNG image                         | .45704 MB          |
| 1024x768 RAW image                         | 1.03 MB Though this is arbitrary I chose Cannon mRaw CR2| 
| HD (1080p) HEVC Video (15 minutes)         | 2250 MB          |
| HD (1080p) Uncompressed Video (15 minutes) | 4500 MB          |
| 4K UHD HEVC Video (15 minutes)             | 9000 MB          |
| 4k UHD Uncompressed Video (15 minutes)     | 18000 MB          |
| Human Genome (Uncompressed)                | 2.9 GB Okay that is really cool! |

(I did not have the full instructions due to differences between blackboard and github)
I ended up working hard on the numbers above so I decided to run with them. I hope they are correct.
I think the big takeaway for this assignment is just seeing how much data we consume.

Assume all videos are 30 frames per second
HEVC stands for High Efficiency Video Coding
See the Wikipedia article on display resolution for information on HD (1080p) and 4K UHD resolutions.

https://en.wikipedia.org/wiki/Display_resolution


#### b. Scaling

Using the estimates for data sizes in the previous part, 
determine how much storage space you would need for the following items.


|                                           | Size     | # HD | 
|-------------------------------------------|---------:|-----:|
| Daily Twitter Tweets (Uncompressed)       | 768 GB (I rounded 1024 gb to 1000 gb per tb.) |   3 hard drives for redundancy   |
| Daily Twitter Tweets (Snappy Compressed)  | 303.24 GB      |   3   |
| Daily Instagram Photos                    | 1.02834x10^14  Bytes   |   6   |
| Daily YouTube Videos                      | 4.5 x 10^12  Bytes   |   3 (compression FTW!)  |
| Yearly Twitter Tweets (Uncompressed)      | 2.8032x10^14  Bytes       |   30 (we have to always round up here)   |
| Yearly Twitter Tweets (Snappy Compressed) | 1.10668x10^14 Bytes       |    12  |
| Yearly Instagram Photos                   | 3.753295x10^16      |   3756 wow thats kind of eye opening   |
| Yearly YouTube Videos                     | 1.6425x10^15       |  165 The power of compression. This was eye opening.   |


For estimating the number of hard drives, assume you are using 10 TB and you are storing the data using the 
Hadoop Distributed File System (HDFS). 
By default, HDFS stores three copies of each piece of data, so you will need to triple the amount storage required.

Twitter statistics estimates 500 million tweets are sent each day. For simplicity, assume each tweet is 128 characters.
https://www.internetlivestats.com/twitter-statistics/

See the Snappy Github repository for estimates of Snappy’s performance.
https://github.com/google/snappy

Instagram statistics estimates over 100 million videos and photos are uploaded to Instagram every day. Assume that 75% of those items are 1024x768 PNG photos.
https://www.omnicoreagency.com/instagram-statistics/

YouTube statistics estimates 500 hours of video is uploaded to YouTube every minute. For simplicity, assume all videos are HD quality encoded using HEVC at 30 frames per second.
https://www.omnicoreagency.com/youtube-statistics/


#### c. Reliability

Using the yearly estimates from the previous part, estimate the number of hard drive failures per year using data from 
Backblaze’s hard drive statistics.
https://www.backblaze.com/b2/hard-drive-test-data.html

For this one I am assuming seagate 10gb drives
I always round up here a dead drive is a dead drive. :)
In this case its best to expect the worst case scenario.


|                                    | # HD | # Failures |
|------------------------------------|-----:|-----------:|
| Twitter Tweets (Uncompressed)      | 30   |       .3997 = 1     |
| Twitter Tweets (Snappy Compressed) | 12   |       .159866 = 1     |
| Instagram Photos                   | 3756   |      50.038 = 51     |
| YouTube Videos                     | 165   |       2.198 = 3     |

#### d. Latency

Provide estimates of the one way latency for each of the following items. 
Please explain how you arrived at the estimates for each item by citing references or providing calculations.
I used https://wondernetwork.com/pings/Los%20Angeles/Amsterdam


|                           | One Way Latency      |
|---------------------------|---------------------:|
| Los Angeles to Amsterdam  | 140.542 ms                 |
| Low Earth Orbit Satellite | 270 ms   According to a tweet from elon musk spacex will reach 20 ms to 40 ms later this year              |
| Geostationary Satellite   | 270 ms                 |
| Earth to the Moon         | 1.3 seconds so 1300 ms                 |
| Earth to Mars             | the orbit of mars makes this have a varied range 3-21 minutes            | 


In python each character is maximum of 4 Bytes
I am assuming UCS-4 encoding which is the largest
128x4 = 512 Bytes

The next two questions I used a online calculator then converted it.
PNG is los-less at 24 bit/pixel
457.04 KB x 1/1000 = .45704 MB

I had two choices for Raw I chose the larger one Cannon mRaw CR2
Though I know Nikon also has raw data images. It makes me wonder what new cellphones use. 
1.03 mb

HEVC or H.265 is a compression format which is an upgrade over H.264 It seems to focus on details in the 
highly detailed parts of the image and does not work as hard where there is less detail. 
According to medium there is a 50% savings on storage when using H.265!

4k is 4x 1080p resolutions, so the numbers scale by 4x the amount!

500 million tweets
(Note for simplicity I am going to round 1024 gb in 1 terrabyte to 1000 gb per tb)

we are assuming 128 characters for each tweet
If we use the largest python encoding that means that 512 Bytes per tweet. 
500,000,000 x 512 Bytes = 256,000,000,000 Bytes x3 for redundancy 768,000,000,000 Bytes

The compression rate I calculated from here
https://blog.openbridge.com/what-is-google-snappy-high-speed-data-compression-and-decompression-f6919f20dce4
They said they had a file that was 5.6 mb and ran snappy and ended up with a file that was 2.4 mb. So about 43%.

768,000,000,000 * .43 = 303,240,000,000 Bytes using snappy

Instagram
100 million uploads daily 75% are PNG photos
There are 1000000 Bytes in 1 MB. So .45704 x 1000000 = 457040 Bytes per photo. 

75 million photos x 457040 = 3.4278x10^13 x 3 for redundancy =1.02834x10^14

since a terabyte is 1e+12
our hard drives are 1.0 x 10^13
3.4278x10^13 does not fit on one hard drive we now need 2 for redundancy we need 6 total

Youtube
500 hours daily
assuming hvec encoding at a rate of 2250 mb per 15 min. So 9000 mb per hour
9000 x 1000000 = 9,000,000,000 Bytes per hour x 500 hours = 4,500,000,000,000 Bytes Daily
9.0 x 10^9 Is less than 1.0 x 10^13 so 1 hard drive (good thing they used compression) which scales to 3


Converting to yearly:

| Yearl Twitter Tweets (Uncompressed)       | 768 GB  |   3  |
7.68 x 10^11/3 x 365 = 9.25x10^13/ 1.0 x 10^13 (size of our hard drives)= 9.24 round up to 10 then mult 3 30


| Yearly Twitter Tweets (Snappy Compressed)  | 303.24 GB      |   3   |

3.0324x10^11 x 365 = 1.10668x10^14/3 (since we want only the part that is on each hard drive) / 1.0 x 10^13 = 3.6889 
The thing that is tricky here is we need 4 hard drives to house the original data then we multiply by 3 = 12 hard drives.

| Yearly Instagram Photos                    | 1.02834 x 10^14    |   6   |

1.02834x10^14/3*365=1.26x10^16/10^13=1251.147 round up 1252 x3 = 3756 Wow thats crazy! 
This is strange I feel like it should be closer to 2600 hard drives here. 
I checked my numbers and I dont see the mistake if there is one.

| Yearly YouTube Videos                      | 4.5 x 10^12    |   3   |

4.5x10^12/3*365=5.475x10^14 /10^13 = 54.75 Round up 55 x 3 = 165

Hard Drive Failures:

For this one I am assuming seagate 10tb drives in the year 2020
16/1201 = .0133 failure rate.

I am going to just multiply these quickly and put them in the table. 










