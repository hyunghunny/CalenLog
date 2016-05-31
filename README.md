# CalenLog
Simple Calendar Log Web App

## How to save logs from Calendar App.
  * Using REST API
     You can save calendar log with below POST Web API

     POST http://adsl.snu.ac.kr:3300/api/labs/:labId/logs/calendars

```
{ "logs" : [
  {
    "logId" : "ux0100",
    "userId" : "u0100",
    "dateFrom": 1428591600000,
    "description": "Meeting",
    "durationMin" : 60,
    "durationMax" : 90
    }
  ]
}
```
  * logs arguments contains an array of the log.
  * Required arguments of a calendar log
    * logId: an unique ID to identify a schedule in the Calendar app. If it is duplicated, the new one will overwrite old one.
    * userId: To figure whom wrote this schedule
    * dateFrom : the start date of a schedule which is transformed to a timestamp which is the number of milliseconds since 1970/01/01. See [JavaScript getTime()] (http://www.w3schools.com/jsref/jsref_gettime.asp)
    * description : the detail of a schedule.
    * durationMin : the minimum time (per minutes) of a schedule
    * durationMax : the maximum time (per minutes) of a schedule
    
## Javascript Wrapper API
* TODO

## Sample code
* TODO
