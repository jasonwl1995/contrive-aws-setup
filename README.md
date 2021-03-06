# contrive-aws-setup
In order to get image upload functioning for contrive app, please follow the following instructions to get AWS S3 set up for full functionality.

This components provide either a file-upload dropzone for users to upload their profile pictures, company logos, and event photos.

## Installation 
``` $ npm install react-dropzone-s3-uploader ```

## Setup
#### 1. Go to: console.aws.amazon.com

#### 2. Register to create an account

#### 3. We need to setup S3 and security permissions:

    3a. To get to S3 from the dashboard, click on All Services to find Storage and click on S3. (Or go to the URL : ‘https://s3.console.aws.amazon.com/s3/')
    
    3b. Click Create Bucket (as of 12/6/2020 - it’s an orange button near the top right corner)
    
    3c. Name your bucket
    
    3d. Select a region (whatever would be closest to your location) and remember what you selected
    
    3e. For now, disregard all the other options and leave the default selections - scroll to the bottom of the page and click Create Bucket
    
![image](https://user-images.githubusercontent.com/71994152/114344673-de439b80-9b25-11eb-9fb1-e088d497dbd1.png)

#### 4. Click on your new bucket and find the Permissions tab:

    4a. The first option is Block public access (bucket setting) - click Edit and choose to turn OFF blocking all public access (so it looks like below). There will be a model that requires you to type confirm - go ahead and do that.
    
![image](https://user-images.githubusercontent.com/71994152/114344914-57db8980-9b26-11eb-818a-667611fb5c30.png)

    4b. Then, still in Permissions, scroll down to find Cross-Origin Resource Sharing (CORS) and click EDIT
    4c. Copy and paste this code into the CORS textbox
    ```
    [
        {
          "AllowedHeaders": [
            "*"
          ],
          "AllowedMethods": [
            "PUT",
            "POST",
            "GET"
          ],
          "AllowedOrigins": [
            "*"
          ],
          "ExposeHeaders": []
      }
    ]
    ```
#### 5. Go to console.aws.amazon.com/iam

    5a. Click on Groups link under Access Management
    
    5b. Click on Create New Group
    
    5c. Name the group (anything of yoru chosing)
    
    5d. The next page should be titled Attach Policy - check the box for AmazonS3FullAccess
    
    5e. The next page should give you the option to Create Group

#### 6. Back on the dashboard page, click on Users link under Access

    6a. Click on Add User

    6b. Name the user (anything of yoru chosing)

    6c. Under Access Type check the Programmatic Access box

    6d. The next page, give this user access to the group we just created

    6e. You can skip the Tags portion

    6f. Review and click Create User
    
    6g. This next page has our access keys!
![image](https://user-images.githubusercontent.com/71994152/114346325-e3561a00-9b28-11eb-994f-d00c16fa041f.png)

#### 7. Click [Show] for the Seceret Accesss Key and copy and paste them into the .env file.
    7a. Copy and paste the your URL of your bucket into the REACT_APP_S3_URL as well.

![image](https://user-images.githubusercontent.com/71994152/114347034-f7e6e200-9b29-11eb-9af4-1bf6412147c2.png)

#### 8. Go to the ImageUpload component in the application and open ImageUpload.jsx. 
    8a. Update this code as needed.
    8b. Change (server: 'http://localhost:5000') as needed to the website you deploy to.
![image](https://user-images.githubusercontent.com/71994152/114349980-3088ba80-9b2e-11eb-8e0b-8a497efd4487.png)

#### 9. Go to the server.js component in the application. 
    9a. Scroll down and find // AWS Configuration //
    9b. Adjust (bucket: 'your-bucket-name') and (region: 'your-region-number') to match your bucket name and region selected.
    
![image](https://user-images.githubusercontent.com/71994152/114349937-28307f80-9b2e-11eb-9099-238ed345de49.png)

## 10. Set up completed
NOW YOU CAN TEST!!
Drag an image to the uploader and it should make a POST request to your bucket, and a GET request to get the preview.
Check your bucket on AWS as well to see that the image has been uploaded.

## HEROKU SETUP
To set up Heroku, type in your terminal
$ heroku create

In a browser go to https://devcenter.heroku.com/articles/s3-upload-node and scroll down to Heroku Setup

![image](https://user-images.githubusercontent.com/71994152/116499225-478b1480-a871-11eb-9b61-27c7a59d9c05.png)

and copy:
$ heroku config:set AWS_ACCESS_KEY_ID=xxx AWS_SECRET_ACCESS_KEY=yyy Insert your own keys and execute in the terminal

To double check, you can go to Heroku.com and click into your app. Go to settings, and under the Config option, click the Reveal Config Vars button. Your keys should be there.

Now we need to add postgres to our Heroku app. In the terminal, paste: $ heroku addons:create heroku-postgresql:hobby-dev
You can go back to the app on Heroku and in the Reveal Config Vars, the new database information should be there.

Back in the code, in our ImageUpload component, we need to update the location of the server. We can just updated the location of your server in the uploadOptions to be an empty object and remove the uploadOptions variable

![image](https://user-images.githubusercontent.com/71994152/116499478-dc8e0d80-a871-11eb-8118-5c2906e67824.png)

Next, we’re going to update this to use an environment variable, instead of harding that s3Url information directly to the component.
Go to your .env file and add 1 more variables, REACT_APP_S3_URL (REACT_APP is required, the rest is for your chosing), and update it with the correct information (example: 'https://contrive.s3.amazonaws.com')

![image](https://user-images.githubusercontent.com/71994152/116499920-f845e380-a872-11eb-9572-3b6ef2110e5f.png)

Back in the component, update the s3Url variable to point to that new .env variable, like the image below:
![image](https://user-images.githubusercontent.com/71994152/116499988-2c210900-a873-11eb-8517-1885a8f58162.png)

(you will need to restart your client and server to get this working!)
Then push to Git and then Heroku $ git push heroku main
Once it has successfully deployed, refresh your heroku app and try to upload an image SUCCESS!!

#### Support
If you have suggestions or issues, please email me at jasonwl_1995@yahoo.com.
