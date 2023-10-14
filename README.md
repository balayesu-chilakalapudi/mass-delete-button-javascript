# mass-delete-button-javascript
mass-delete-button-javascript
use this javascript script code for deleting number of records on an object listview.

```
{!REQUIRESCRIPT("/soap/ajax/21.0/connection.js")}
 
//Add ObjectType, on which object you want to add the list view button .
var records = {!GETRECORDIDS($ObjectType.Lead)
};
 
if (records[0] == null) {
    alert("Please select at least one record to delete.")
} else {
 
    var opt = confirm("Are you sure you want to delete selected records ?");
    if (opt == true) {
        var errors = [];
        var result = sforce.connection.deleteIds(records);
        if (result && result.length) {
            var nFailed = 0;
            var nSucceeded = 0;
            for (var i = 0; i < result.length; i++) {
                var res = result[i];
                if (res && res.success == 'true') {
                    nSucceeded++;
                } else {
                    var es = res.getArray("errors");
                    if (es.length > 0) {
                        errors.push(es[0].message);
                    }
                    nFailed++;
                }
            }
            if (nFailed > 0) {
                alert("Failed: " + nFailed + " and Succeeded: " + nSucceeded + " n Due to: " + errors.join("n"));
            } else {
                alert("Number of deleted records: " + nSucceeded);
            }
        }
        window.location.reload();
    }
}
```

