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
press **y** if prompted.

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
We will create two different buckets for S3. One will be our website without 'www' (**non-www**) and another with 'www' (**www**).
The **www** website will act as the authority which will host all of our files while the **non-www** is for redirection.

Create the bucket.

![stg2 1](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/34f290ed-5c40-462d-88fd-8c4e51262333)

Name the bucket corresponding to our website url. Select ```US East(N. Virginia) us-east-1``` as our **AWS Region**.
Press on **Create Bucket**.

![stg2 2](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/55a1e01e-3e35-4fdf-aad7-0d8dad9268c9)
![stg2 3](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/e339f12b-4a85-4fa6-a965-4f78e89e0746)

Next, create the second bucket that corresponds with the website name. Select ```US East(N. Virginia) us-east-1``` as our **AWS Region**
Press on **Create Bucket**.

![stg2 4](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/071597e5-6c29-4c3d-9671-16f15b16b805)
![stg2 5](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/e1d417d2-ca92-4eb6-9c05-2c50153eccef)

Once those are both created select the **www** and upload the React files to it. 

![stg2 6](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/787d1432-779e-433a-934d-f9a7de4239fb)

Select **Upload**. Press on **Add files** and add all the files in the ```Demo-App --> build``` directory. 

![stg2 7](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/c6d47422-ced6-467b-b290-f12576e10aec)
![stg2 8](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4b61d5c9-746a-4c4b-aa95-34e52338d70f)
![stg2 9](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/849cb24d-6e33-477e-b568-9b83e72ae539)

Once added select **Add folder** and add the static folder.
![stg2 10](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/f6170303-b67a-4bb5-aa84-8dbc020d8426)
![stg2 11](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/2888a5c1-19a6-4f89-bfc1-cba9ed85a74d)

All the files and folders should be in the table ready for upload.
Press on **Upload**.

![stg2 13](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/80e933ec-c2d7-40e2-a017-a20a41a0c279)

Once the upload succeeds you can press **Close**.

![stg2 14](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/321c898e-8601-44fb-83a3-d5a897ca2632)

Head to the **www** bucket and press on **Permissions**.

![stg2 15](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/e56d64b7-eb3c-450e-8ee5-5320eccbd784)

Head to ```Block public access (bucket settings)``` and press on **Edit**.

![stg2 16](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/881776fd-5bd0-464b-b884-674edc718513)

Uncheck the ```Block all public access``` setting and press on **Save Changes**.

![stg2 17](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/3ffc68ca-5a26-44a8-ba88-4297e52ecfae)

Enter in ```confirm``` and press on **Confirm** 

![stg2 18](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4298409c-d05c-4729-876c-f1d1fb1e398c)

Head to ```Bucket Policy``` and press on **Edit** 

![stg2 19](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/7f63b87a-ea58-41dc-b1a3-807601e30f28)

Paste in this bucket policy and replace ```Bucket-Name``` with the name of the corresponding bucket that has the **www** url. 

Note: *The bucket-policy* file is attached to this repository: https://github.com/Michael-DTran/React-Domain-Proj/blob/6dbb0515bb168820d4e9ca365771f4d0a1dcaed0/bucket-policy

Press on **Save changes** when complete. 
Note: *Ignore the API Error*

![stg2 21](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/8227f8b6-768f-4dad-bb3f-592c2d6ff737)

![stg2 22](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4b2697fa-2e82-4576-84d9-831577462870)

To do a manual check head to the **www** bucket and click on ```index.html```


![stg2 23](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/1a35ab78-261f-418b-b43a-bc5cf1db889c)

![stg2 24](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/ddc429df-0ed1-4594-b3e9-d1a3d6ec5731)

Enter in the ```Object URL```

![stg2 25](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/52b9ce6d-b600-4432-915a-9c52cdcf1f48)

The page should be blank since you don't have any of the javascript or css files added on to it but the React App should still open in this file. 
This test is to show that the bucket is in fact public and that you can access the files.

![stg2 26](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/96527be6-22b7-4ff0-8b20-429b27721161)

Head back to the **www** and press on the ```Properties``` tab. Scroll down until you see ```Static website hosting```. Press on **Edit**.

![stg2 27](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/8d379ef8-a0bc-48e7-b3a5-4e53ae8d1d33)

![stg2 28](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/c5b50871-61e3-49a9-9802-042a1172f783)


Change ```Static website hosting```to ```enable``` and ```Hosting type``` to ```Host a static website```. For ```Index document``` type in the ```index.html``` file.

![stg2 29](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/b3e2c992-687b-4544-9bb7-77253f601483)

Press on **Save changes**.

![stg2 30](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/13b6175e-b2e3-4224-94ae-06778c4117d2)

Head to the **non-www** bucket and press on the ```Properties``` tab. Scroll down until you see ```Static website hosting```. Press on **Edit**.

![stg2 31](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/0c2c7b07-462c-4a17-86ad-a6c4e2367a01)

![stg2 32](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/5afb7fd6-d6e5-452c-890b-7b4803a88493)

Change ```Static website hosting```to ```enable``` and ```Hosting type``` to ```Redirect requests for an object```. Under ```Host name``` enter in the bucket with *www*. 
Under ```Protocol - Optional``` change it to ```http``` for now. When we set up the CF distribution we will change it to https later on in this project. 
Click on **Save changes**.

![stg2 33](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/daf638a3-14de-4702-9f6b-7f40a3d092bc)


# Stage 3: Make Route 53 Modifications









