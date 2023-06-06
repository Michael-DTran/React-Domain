# React-Domain-Proj

# Overview

![React-Arc](https://github.com/Michael-DTran/React-Domain/assets/112426094/a783c826-c47c-47f9-ac58-65e8d35e290b)

In this project I will be creating a React App and upload those files to a S3 Bucket to host a static website. Once uploaded I will be making a domain name service under Route 53 (R-53). Once registered I will create a certificate on AWS Certificate Manager (ACM) on a Cloudfront (CF) distribution to enable caching and https. Once those are all set up I will then verify that everthing works. 

# Stage 1: Create React App 
Make a directory folder to store all the React files.
Open Windows Powershell and navigate to your file directory 
Enter the following commands: 
Note: *Make sure you have npm installed*

``` npx create-react-app demo-app ```

![stg1 1](https://github.com/Michael-DTran/React-Domain/assets/112426094/4e4dc74b-a9be-495c-9e43-a118bda9708b)

Once installed enter into the demo-app

``` cd demo-app ```
``` npm start ```

![stg1 3](https://github.com/Michael-DTran/React-Domain/assets/112426094/8e043e92-7105-4389-9188-97b39a1bb7d7)

The react app should start up on a seperate window. This is what I will be deploying on the S3 website. You can close this window afterwards. 

![stg1 4](https://github.com/Michael-DTran/React-Domain/assets/112426094/b0d2e2ad-cbec-4c66-917f-92a0a3b4e0dd)

Bundle all of the files together so they can be deployed in a production environment.

```npm run build``` 
press 'y' if prompted.

![stg1 6](https://github.com/Michael-DTran/React-Domain/assets/112426094/91310a47-46b2-4402-bb5c-ca51b5bad7d2)

All the files will be batched into some static files. Enter in ```demo-app --> build``` 
You should see the index.html, logos, manifest, and robots files

![stg1 7](https://github.com/Michael-DTran/React-Domain/assets/112426094/3ca60703-baba-4a70-8b49-64ed6c4bf3e4)
![stg1 8](https://github.com/Michael-DTran/React-Domain/assets/112426094/8ecf1bc9-5b33-44b7-9b9e-a5d0d59fdd4b)
![stg1 9](https://github.com/Michael-DTran/React-Domain/assets/112426094/4da83375-2ae9-42b7-b6b5-dcdd0c976dbf)


If you delve deeper into ```demo-app --> build --> static```
You should see the css, javascript, and media files. All of these files will be uploaded to S3 to host the website. You can now close Powershell as you do not need it anymore.

![stg1 10](https://github.com/Michael-DTran/React-Domain/assets/112426094/f5cde2b6-13f0-41c2-a016-52a1d383bd0e)

# Stage 2: Create the S3 Bucket
Head to S3: https://s3.console.aws.amazon.com/s3
We will create two different buckets for S3. One will be our website without 'www' (*non-www*) and another with 'www' (*www*).
The *www* website will act as the authority which will host all of our files while the *non-www* is for redirection.

Create the bucket.

Name the bucket corresponding to our website url. Select *US East(N. Virginia) us-east-1* as our **AWS Region**.

Press on **Create Bucket**.


Next, create the second bucket that corresponds with the website name. Select *US East(N. Virginia) us-east-1* as our **AWS Region**

Press on **Create Bucket**.

Once those are both created select the *www* and upload the React files to it. 

Select **Upload**. Press on **Add files** and add all the files in the ```Demo-App --> build``` directory. Do not add the ```static``` folder yet.
Once added select **Add folder** and add the static folder.

All the files and folders should be in the table ready for upload.

Press on **Upload**.

Once the upload succeeds you can press **Close**.

Head to the *www* bucket and press on **Permissions**.

Head to *Block public access (bucket settings) and press on **Edit**.

Uncheck the *Block all public access* setting and press on **Save Changes**.

Enter in *confirm* and press on **Confirm** 

Head to *Bucket Policy* and press on **Edit** 

Paste in this bucket policy and replace *Bucket-Name* with the name of the corresponding bucket that has the *www* url. 
Note: *The bucket-policy* file is attached to this repository. 
Press on **Save changes** when complete. 

Once completed. your *www* bucket should have an icon saying that it is *public* with your *non-www* reading *bucket and objects not public*

To do a manual check head to the *www* bucket and click on *index.html*

Enter in the *Object URL*

The page should be blank since you don't have any of the javascript or css files added on to it but the React App should still open in this file. 
This test is to show that the bucket is in fact public and that you can access the files.

Head back to the *www* and press on the **Properties** tab. Scroll down until you see *Static website hosting*. Press on **Edit**.
Change *Static website hosting* to *enable* and *Hosting type* to *Host a static website*. For *Index document* type in the *index.html* file.
Press on **Save changes**.


Head to the *non-www* bucket and press on the **Properties** tab. Scroll down until you see *Static website hosting*. Press on **Edit**.
Change *Static website hosting* to *enable* and *Hosting type* to *Redirect requests for an object*. Under *Host name* enter in the bucket with *www*. 
Under *Protocol - Optional* change it to *http* for now. When we set up the CF distribution we will change it to https later on in this project. 
Click on **Save changes**.












