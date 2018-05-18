# Invoking a Lambda Function Code<a name="browser-invoke-lambda-function-full"></a>

Here is the complete HTML and browser script for the slot machine example\.

```
<!doctype html>
<html>
<head>
<meta charset="UTF-8">
<title>Emoji Slots</title>
<link href="emojislots.css" rel="stylesheet" type="text/css">
<script src="https://sdk.amazonaws.com/js/aws-sdk-2.243.1.min.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
<script type="text/javascript">
   // Configure AWS SDK for JavaScript
   AWS.config.update({region: 'REGION'});
   AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: 'IDENTITY_POOL_ID'});

   var pullReturned = null;
   var slotResults;	
   var isSpinning = false;
	
   // Prepare to call Lambda function
   var lambda = new AWS.Lambda({region: 'REGION', apiVersion: '2015-03-31'});
   var pullParams = {
      FunctionName : 'slotPull',
      InvocationType : 'RequestResponse',
      LogType : 'None'
   };
		
   function pullHandle() {
      if (isSpinning == false) {
         // Show the handle pulled down
         slot_handle.src = "lever-dn.png";
      }
   }
	
   function initiatePull() {
      // Show the handle flipping back up
      slot_handle.src = "lever-up.png";	
      // Set all three wheels "spinning"
      slot_L.src = "slotpullanimation.gif";
      slot_M.src = "slotpullanimation.gif";
      slot_R.src = "slotpullanimation.gif";
      // Set app status to spinning
      isSpinning = true;
      // Call the Lambda function to collect the spin results
      lambda.invoke(pullParams, function(err, data) {
         if (err) {
            prompt(err);
         } else {
            pullResults = JSON.parse(data.Payload);
            displayPull();
         }
      });	
   }
	
   function displayPull() {
      isSpinning = false;
      if (pullResults.isWinner) {
         winner_light.visibility = visible;
      }
      $("#slot_L").delay(4000).attr("src", pullResults.leftWheelImage.file.S);
      $("#slot_M").delay(6500).attr("src", pullResults.midWheelImage.file.S);
      $("#slot_R").delay(9000).attr("src", pullResults.rightWheelImage.file.S);
   }

</script>
</head>
<body>
   <div id="appframe">

   <img id="slot_L" src="puppy.png" height="199" width="80" alt="slot wheel 1"/>
   <img id="slot_M" src="puppy.png" height="199" width="80" alt="slot wheel 2"/>
   <img id="slot_R" src="puppy.png" height="199" width="80" alt="slot wheel 3"/>
   <img id="winner_light" src="winner.png" height="48" width="247" alt="winner indicator"/>

   </div>
</body>
</html>
```