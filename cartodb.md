# Working with CartoDB

## Create an account

Go to https://cartodb.com/signup to create an account (if you don't already have one). Free accounts allow you to upload 50 MB of data, but keep in mind that all your data and maps will be public.

## Login

Once logged in, you will see your private dashboard. This is where you can upload data, create maps and manage your account.

At the top left is a dropdown menu where you can select either **Maps** (default) or **Datasets**. At the top right, there is a tab that wil take you to documentation for the CartoDB Editor.

You also have a public page at https://user.cartodb.com/maps. All the maps that you create will be visible there.

## Uploading data

For this exercise, we're going to use an extremly simple dataset, [Edinburgh Trees with a Story](http://edinburghopendata.info/dataset/edinburgh-s-trees-with-a-story), from the Edinburgh Open Data Portal.

If you navigate to the Datasets tab, you should see a screen like the following:

![](images/carto1.tiff)

Click on the link in the centre of the screen that says **connect datasets**, and you'll then see a screen like this:

![](images/carto2.tiff)

The easiest way to upload the data we want is to copy the following horrible URL, and paste it into the little **SUBMIT** box on the CartoDB **Connect dataset** page.

```
http://edinburghopendata.info/dataset/8f46d456-3115-41f0-bef5-f45de1909608/resource/e61ee62b-e58a-413b-8079-f9b3ee4e219a/download/edinburghstreeswithastory.csv
```

Hit the **SUBMIT** button, and then the button that says **CONNECT DATASET**. 

After a moment or two, you should get a little pop-up window like this:
![](images/carto3.tiff), and clicking on **SHOW** will take you to this screen:
![](images/carto4.tiff).

## Editing the data

At this point, we hit a minor difficulty. The second column in the table &mdash; the one labeled `the_geom` &mdash; is where CartoDB expects to find the location coordinates (i.e. latitude and longitude). However, that information is somewhere else, namely in the column labeled `location`. 
Unfortunately, this isn't the end of our problems! The `location` column has the geographical coordinates as a single string, but CartoDB requires the latitude and longitude to each have their own column. 

Here's how we fix it. First, click on the little downwards triangle at the top of any column, and you'll get a menu like this:

![](images/carto6.tiff)

Select **Add new column**, and give it the name `latitude`. Now repeat this, but call the second new column `longitude`.

To fix this, click on the **Edit** button on the top righthand side of the screen, then choose the **Georeference** option.

![](images/carto5.tiff)

Click on the box that says **Select column**, and choose **Location**. 

```
UPDATE edinburghstreeswithastory SET latitude = split_part(location, ', ', 1)
```

```
UPDATE edinburghstreeswithastory SET longitude = split_part(location, ', ', 2)
```
