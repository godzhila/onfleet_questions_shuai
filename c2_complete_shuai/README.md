## Challenge 2

a) I am 1 billion seconds old today. On what day was I born?

When I did the calculation : 2020-01-17 17:42:23

31years 259 day 1 hour 47 mins and 40 something seconds ago
 
So somewhere in year 1988, May something rough estimate, let along getting the day of the week, and/or the details.  

However, Java Date util has some nifty stuffs where it can calculate time given the seconds in the standard format. so i did that real quick 

using Groovy:
```
import java.util.Date

def cur_date = new Date();
def bil_date = new Date();

cur_date.getSeconds()//(cur_date.getSeconds());
bil_date.setSeconds(bil_date.getSeconds()- 1000000000);

System.out.println("old Value is: "+ cur_date);
System.out.println("New Value is: "+ bil_date);
```

old Value is: Fri Jan 17 17:59:39 PST 2020
New Value is: Tue May 10 16:12:59 PDT 1988


b) Pretend it is Wednesday, October 24, 2018. A Swiss dispatcher, Lara, will be coordinating the schedule for the next 1 week of deliveries for a team of drivers in Jordan. She can only schedule deliveries the day before they are set to be delivered. Assuming that Lara is working from her local timezone, the drivers work in theirs, and you are in the Pacific Timezone, how would you advise Lara to set the delivery completion(???) times over each of the following 7 days?

**{answer}**
Switzland(GMT+1) normally is an hour behind Jordan(GMT+2) and that will be the difference of time between the two countries, but this sepcific day was chosen i belive becuase it poses some DST situation.  Not sure what deliver completion means, but assuming it's talking about when the deliver have to be completed by, then I would say set it at 10:50pm at night Switzerland time, and only on Friday, 26th of that week she will have to set it earlier(maybe 9:50pm Swiss time) because Switzerland and Jordon would be on the same time zone that Saturday(GMT+2), and resume to 10:50pm on Saturday and continue to Sunday since Switzerland falls back on that Sunday, 28th.  

Simply align your timezone to the local driver is what I would suggest on a very high level.  
**{answer}**

Assume that a Jordanian customer from that set of deliveries complains that he didn't receive the expected text/SMS message on Saturday, for a delivery at 16:00, local time. Assuming that the telephony logs are in Pacific timezone, what timestamp would you look for in the logs?

**{answer}**
First I find the two timezones involved, which is Pacific(GMT-8) and Jordan(GMT+2) and calculate the differences.  However, then I remembered that US oberses DST which would sometimes put Pacific into GMT-7.  I am unsure if Jordan observe DST because alot of the countries in the world doesn't(ie. Taiwan). So I went on wikipedia to find out if Jordan does, and apparently it does, and sure enough they would do "fall back" on Oct 27th, 2018, while Pacific would do it's fall back on Nov.  That means the time difference would not be a simple subtraction between GMT+2 and GMT-8, it would actually be GMT+2 and GMT-7, which is 9hours.  Given the problem arises on Jordan time, 16:00(GMT+2), I would check in the logs(in Pacific time GMT-7) at around 7am, plus or minus up to an hour to see what may have been the problem.   
**{answer}**

