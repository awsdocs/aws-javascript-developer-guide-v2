# Getting Started in a Browser Script<a name="getting-started-browser"></a>

To start using the SDK for JavaScript in browser scripts, create a simple browser\-based app that authenticates users using Amazon Cognito Identity and Facebook login\. After logging in, the user gets temporary AWS credentials and assumes the pre\-specified AWS Identity and Access Management \(IAM\) role\. The role policy allows uploading and listing objects in Amazon Simple Storage Service \(Amazon S3\)\.

For information about downloading and installing the AWS SDK for JavaScript, see [Installing the SDK for JavaScript](installing-jssdk.md)\.



## Step 1: Creating and Configuring an Amazon S3 Bucket<a name="getting-started-browser-create-bucket"></a>

In this exercise, you will use an Amazon S3 bucket both to store the objects you upload in the application and to host the HTML page of the application itself\. Cross\-origin resource sharing, or CORS, must be configured on the Amazon S3 bucket to be accessed directly from JavaScript in the browser\. For more information about CORS, see [Cross\-Origin Resource Sharing \(CORS\)](cors.md)\.

**To create and configure the Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\. Give the bucket a unique name\. Note the bucket name and region for later use\. Choose **Next**\.

1. In the **Set properties** panel under **Manage public permissions**, choose **Grant public read access to this bucket**\. Choose **Next**\.

1. Choose **Create bucket**\. Choose the new bucket in the console\.

1. In the pop\-up\-dialog, choose **Permissions**  
![\[CORS Configuration Editor in Amazon S3 for setting CORS configuration of a bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/s3-bucket-dialog.png)

1. In the **Permissions** tab, choose **CORS Configuration**\. The console displays the **CORS configuration editor**\.  
![\[CORS Configuration Editor in Amazon S3 for setting CORS configuration of a bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/s3-cors-config.png)

1. Copy the following XML into the **CORS Configuration Editor** and then choose **Save**\.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01">
      <CORSRule>
         <AllowedOrigin>*</AllowedOrigin>
         <AllowedMethod>GET</AllowedMethod>
         <AllowedMethod>PUT</AllowedMethod>
         <AllowedMethod>POST</AllowedMethod>
         <AllowedMethod>DELETE</AllowedMethod>
         <AllowedHeader>*</AllowedHeader>
      </CORSRule>
   </CORSConfiguration>
   ```

## Step 2: Creating the Facebook App and Getting the App ID<a name="getting-started-browser-facebook"></a>

To complete this project, you must first obtain a Facebook App ID to use for Facebook login with Amazon Cognito Identity\.

**To obtain a Facebook App ID**

1. Go to [https://developers\.facebook\.com](https://developers.facebook.com) and log in with your Facebook account\.

1. From the **My Apps** drop\-down menu, choose **Add a New App**\.  
![\[Menu for adding a new App ID in Facebook Developer Console\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/facebook-add-app.png)

1. Configure the Facebook app settings to specify the app display name and your contact email\. Note the app ID provided by Facebook, which you will include in the browser script later\.

1. On the Facebook **Products** page, select **Facebook Login** then **Set Up**\. Choose **Web** for the platform where the application will run\.

1. To specify the **Site URL**, type **https://*YOUR\-BUCKET\-NAME*\.s3\.amazonaws\.com/**\. Then choose **Save**\.

For more information about adding Facebook Login to a website, see [https://developers\.facebook\.com/docs/facebook\-login](https://developers.facebook.com/docs/facebook-login)\.

## Step 3: Creating an IAM Role to Assign Users<a name="getting-started-browser-iam-role"></a>

To prevent users from overwriting or changing other users' objects, users are authenticated using Facebook Login\. Each user who logs in has a unique identity that becomes part of the Amazon S3 key assigned to uploaded objects\. To protect objects uploaded by other users, use an IAM role assigned user\-specific write permissions at the prefix level with an IAM role policy\. For more information on IAM roles, see [Creating a Role to Delegate Permissions to an AWS Service](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the IAM User Guide\.

**To create the IAM role and assign users**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies** and then choose **Create Policy**\. For **Create Your Own Policy**, choose **Select**\.

1. Name your policy \(for example **JavaScriptSample**\) and then copy the following JSON policy to the **Policy Document** text box\. Replace the two instances of *YOUR\_BUCKET\_NAME* with your actual bucket name and then choose **Create Policy**\.

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Action": [
               "s3:PutObject",
               "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR_BUCKET_NAME/facebook-${graph.facebook.com:id}/*"
            ],
            "Effect": "Allow"
         },
         {
            "Action": [
               "s3:ListBucket"
            ],
            "Resource": [
               "arn:aws:s3:::YOUR_BUCKET_NAME"
            ],
            "Effect": "Allow",
            "Condition": {
               "StringEquals": {
                  "s3:prefix": "facebook-${graph.facebook.com:id}"
                }
             }
          }
       ]
   }
   ```

1. In the IAM console navigation pane, choose **Roles** and then choose **Create New Role**\.

1. Choose **Role for identity provider access** and select **Grant access to web identity providers**\.

1. For **Identity Provider**, choose **Facebook**\. For **Application ID**, type your app ID and then choose **Next Step**\.

1. On the **Verify Role Trust** page, choose **Next Step**\.

1. On the **Attach Policy** page, choose the policy you just created and then choose **Next Step**\.

1. Enter a role name and choose **Create Role**\.

## Step 4: Creating the HTML Page and Browser Script<a name="getting-started-browser-create-html"></a>

The sample app consists of a single HTML page that contains the user interface and browser script\. Create an HTML document and copy the following contents into it\.

```
<!DOCTYPE html>
<html>
<head>
    <title>AWS SDK for JavaScript - Sample Application</title>
    <meta charset="utf-8">
    <script src="https://sdk.amazonaws.com/js/aws-sdk-2.196.0.min.js"></script>
</head>

<body>
    <input type="file" id="file-chooser" />
    <button id="upload-button" style="display:none">Upload to S3</button>
    <div id="results"></div>
    <div id="fb-root"></div>
    <script type="text/javascript">
        var appId = 'YOUR_APP_ID';
        var roleArn = 'YOUR_ROLE_ARN';
        var bucketName = 'YOUR_BUCKET_NAME';
        AWS.config.region = 'YOUR_BUCKET_REGION';

        var fbUserId;
        var bucket = new AWS.S3({
            params: {
                Bucket: bucketName
            }
        });

        var fileChooser = document.getElementById('file-chooser');
        var button = document.getElementById('upload-button');
        var results = document.getElementById('results');

        button.addEventListener('click', function () {
            var file = fileChooser.files[0];
            if (file) {
                results.innerHTML = '';
                //Object key will be facebook-USERID#/FILE_NAME
                var objKey = 'facebook-' + fbUserId + '/' + file.name;
                var params = {
                    Key: objKey,
                    ContentType: file.type,
                    Body: file,
                    ACL: 'public-read'
                };

                bucket.putObject(params, function (err, data) {
                    if (err) {
                        results.innerHTML = 'ERROR: ' + err;
                    } else {
                        listObjs();
                    }
                });
            } else {
                results.innerHTML = 'Nothing to upload.';
            }
        }, false);

        function listObjs() {
            var prefix = 'facebook-' + fbUserId;
            bucket.listObjects({
                Prefix: prefix
            }, function (err, data) {
                if (err) {
                    results.innerHTML = 'ERROR: ' + err;
                } else {
                    var objKeys = "";
                    data.Contents.forEach(function (obj) {
                        objKeys += obj.Key + "<br>";
                    });
                    results.innerHTML = objKeys;
                }
            });
        }
        /*!
         * Login to your application using Facebook.
         * Uses the Facebook SDK for JavaScript available here:
         * https://developers.facebook.com/docs/javascript/quickstart/
         */

        window.fbAsyncInit = function () {
            FB.init({
                appId: appId
            });

            FB.login(function (response) {
                bucket.config.credentials = new AWS.WebIdentityCredentials({
                    ProviderId: 'graph.facebook.com',
                    RoleArn: roleArn,
                    WebIdentityToken: response.authResponse.accessToken
                });
                fbUserId = response.authResponse.userID;
                button.style.display = 'block';
            })
        };

         // Load the Facebook SDK asynchronously
        (function (d, s, id) {
            var js, fjs = d.getElementsByTagName(s)[0];
            if (d.getElementById(id)) {
                return;
            }
            js = d.createElement(s);
            js.id = id;
            js.src = "https://connect.facebook.net/en_US/all.js";
            fjs.parentNode.insertBefore(js, fjs);
        }(document, 'script', 'facebook-jssdk'));
    </script>
</body>
</html>
```

Before you can run the example, replace `YOUR_APP_ID`, `YOUR_ROLE_ARN`, `YOUR_BUCKET_NAME`, and `YOUR_BUCKET_REGION` with the actual values for the Facebook app ID, IAM role ARN, Amazon S3 bucket name, and bucket region\. You can find the Amazon Resource Name \(ARN\) of your IAM role in the IAM console by selecting the role and then choosing the **Summary** tab\.

Name the HTML file to `index.html` then upload it to the Amazon S3 bucket\.

## Step 5: Running the Sample<a name="getting-started-browser-run-sample"></a>

To run the sample app, type the following URL into a web browser\.

```
https://YOUR-BUCKET-NAME.s3.amazonaws.com/index.html
```