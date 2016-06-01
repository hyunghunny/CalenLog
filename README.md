# CalenLog
Simple Calendar Log Web App

## How to save logs from Calendar App.
  * Using REST API
  
    You can save calendar log with below POST Web API:

     POST http://adsl.snu.ac.kr:3300/api/labs/ux/logs/calendars

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
   
  * REST API is closely opened.
  
## Javascript Wrapper API
* You can add the schedule with the following function.
```
// Add a schedule to the logs
// CAUTION: this function depends on the allLogs global varible
var userId = "testId";
var allLogs = [];  // contains all the schedules 
function addSchedule(id, dateFrom, description, durationMin, durationMax) {
    var scheduleObj = {
        "userId" : userId
    };

    // simple sanity check - arguments availablity only
    if (id != null && dateFrom != null && description != null && durationMin != null && durationMax != null) {
        scheduleObj.logId = id;
        scheduleObj.dateFrom = dateFrom;
        scheduleObj.description = description;
        scheduleObj.durationMin = durationMin;
        scheduleObj.durationMax = durationMax;

        allLogs.push(scheduleObj);

    } else {
        console.log('Invalid schedule log');
    }
}
```
  * *allLogs* is a global variable which is referred in addSchedule(). it is high risky implementation to rapid coding (Frankly no head coding)
  * *userId* is a gloval variable which identify the user of calendar app. we discuss it later.
  * See the REST API description above to figure the arguments of addSchedule()
  * This function depend on jQuery for ajax operation
* You can save all schedule logs by calling the following function.

```
// Save all schedules by calling Web API
function saveSchedules(logs, url) {
    var postUrl = baseUrl + 'api/labs/ux/logs/calendars';
    var payload = {};
    payload.logs = logs;
    console.log(JSON.stringify(payload)); // show payload to debug
    $.ajax({
        'url': postUrl,
        'type': 'post',
        'contentType': "application/json",
        'dataType': 'json',
        'statusCode': {
            '202': function (data) {
                alert('The message is posted successfully');
                updateMessages();
            }
        },
        'data': JSON.stringify(payload)
    });
}
```
* *logs* argument requires the all schedule logs (which is the global variable *allLogs* above)
* *url" requries 'http://147.47.123.199:3300/'

## Sample code
* You can simply add the schdules and save them as follows:
```
addSchedule("test01", (new Date("2016-6-1 10:30")).getTime(), "ICST presentation", 75, 90);
addSchedule("test02", (new Date("2016-6-3 10:30")).getTime(), "IDA lecture", 150, 200);

saveSchedules(allLogs, baseUrl);
```
