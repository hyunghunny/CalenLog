# CalenLog
Simple Calendar Log Web App

## How to save logs from Calendar App.
  * Using REST API
  
    You can save calendar log with below POST Web API:


```
POST /labs/ux/logs/calendars

{ 
  "id" : "someone",
  "schedules" : [
  {
    "weekday" : "Monday",
    "top" : 385,
    "height1": 70,
    "height2": 70
    }
  ]
}
```
  * logs arguments contains an array of the log.
  * Required arguments of a calendar log
    * id: To figure whom wrote this schedule
    * schedules : scedule object array
      * weekday : Sunday to Saturday within the experiment 
      * top : the top location of a schedule  
      * height1 : basic box height
      * height2 : extra box height
   
  * REST API is available now.
  
## Javascript Wrapper API
* You can use following export.js functions to export via REST API.
* You can just use the export.js in this project 
```
var allSchedules = []; // global variable to save all schedules
var baseUrl = '../'; // for testing only

function extractSchedules(timeBox) {
    allSchedules = []; // clean up
    
    for (var i = 0; i < timeBox.length; i++) {
        var dayBox = timeBox[i];
        switch (i) {  // boxes allocated with weekday's order.          
            case 0:
                if (dayBox.length > 0) extractInfo('Sunday', dayBox);
                break;
            case 1:
                if (dayBox.length > 0) extractInfo('Monday', dayBox);
                break;
            case 2:
                if (dayBox.length > 0) extractInfo('Tuesday', dayBox);
                break;
            case 3:
                if (dayBox.length > 0) extractInfo('Wednesday', dayBox);
                break;
            case 4:
                if (dayBox.length > 0) extractInfo('Thursday', dayBox);
                break;
            case 5:
                if (dayBox.length > 0) extractInfo('Friday', dayBox);
                break;
            case 6:
                if (dayBox.length > 0) extractInfo('Saturday', dayBox);
                break;
            default:
                console.log('Wrong timebox!')
                break;
        }
    }
}
// extract the box's top and two height
function extractInfo(weekday, dayBoxes) {
    
    for (var i = 0; i < dayBoxes.length ; i++) {
        var dayBox = dayBoxes[i];

        var schedule = {
            "weekday": weekday,
            "top": ($(dayBox).position()).top,
            "height1": ($(dayBox).find('.t_box')).height(),
            "height2": ($(dayBox).find('.t_box2')).height()
        }

        allSchedules.push(schedule);
    }
}

// Export all schedules by calling Web API
function exportSchedules(userId, logs) {
    var postUrl = baseUrl + 'api/labs/ux/logs/calendars';
    var payload = {};
    payload.id = userId;
    payload.schedules = logs;
    console.log(JSON.stringify(payload)); // show payload to debug
    $.ajax({
        'url': postUrl,
        'type': 'post',
        'contentType': "application/json",
        'dataType': 'json',
        'statusCode': {
            '202': function (data) {
                alert('It have been exported successfully');
                
            }
        },
        'data': JSON.stringify(payload)
    });
}
```
  * *allSchedules* is a global variable which is referred in exportSchedule(). it is high risky implementation to rapid coding (Frankly no head coding)
  * *userId* parameter of exportSchedules() is a variable which identify the user of calendar app.
  * See the REST API description above to figure the arguments of exportSchedules()
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
<!-- XXX: below export.js required to export schedules -->
<script src="export.js" ></script>
<script>
...
var schedule_box = [[], [], [], [], [], [], []]; // XXX: this variable MUST be required to save schedule.

	 // XXX:below code export your schedules
		extractSchedules(schedule_box);
		console.log(JSON.stringify(allSchedules)); // for debugging use

		if (allSchedules.length > 0 && confirm("Export your schedule? ") == true) {
            
		    var userId = "testUser0010"; // XXX: this user ID MUST be changed appropriately.

		    exportSchedules(userId, allSchedules);
		}
 // XXX:end of added code
 ...
 schedule_box[date_num].push(tempbox_t); // XXX: this code MUST be required to save the schedule for exporting. 
        
```
  * See the updated index.html in this project
