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

The easiest way to upload the data we want is to copy the following complicated URL, and paste it into the little **SUBMIT** box on the CartoDB **Connect dataset** page. You may have to scroll the mini-window to the right to get all of it.

```
http://edinburghopendata.info/dataset/8f46d456-3115-41f0-bef5-f45de1909608/resource/e61ee62b-e58a-413b-8079-f9b3ee4e219a/download/edinburghstreeswithastory.csv
```

Hit the **SUBMIT** button, and then hit the button that says **CONNECT DATASET**. 

After a moment or two, you should get a little pop-up window like this:

![](images/carto3.tiff), 

and clicking on **SHOW** will take you to this **DATA VIEW** screen:

![](images/carto4.tiff).

## Fixing the Georeferencing

At this point, we hit a minor difficulty. The second column in the table &mdash; the one labeled `the_geom` &mdash; is where CartoDB expects to find the location coordinates (i.e. latitude and longitude). However, that information is somewhere else, namely in the column labeled `location`. 
Unfortunately, this isn't the end of our problems! The `location` column has the geographical coordinates as a single string, but CartoDB requires the latitude and longitude to each have their own column. 

### Add new columns

Here's how we fix it. First, click on the little **add column** icon at the very bottom of the toolbar on the right of the CartoDB window:

![](images/carto6a.tiff)

This will create a new column for you. Give it the name `latitude`. Now repeat this step, but call the second new column `longitude`.

### Copy data to the new columns

We're now going to use some SQL code to separate out the two components (latitude and longitude) of information in the `location` column and copy it to the relevant new column. 

To begin with, click on the little **sql** button on the righthand toolbar:

![](images/carto7.tiff)

This will open a new window. Paste in the following code, and hit the **Apply query** button.

```
UPDATE edinburghstreeswithastory SET latitude = split_part(location, ', ', 1)
```
![](images/carto9.tiff)

This should add new information to the `latitude` column so that the column looks like this:

![](images/carto8.tiff)

Now do the same again, except paste the following code into the **sql** window:

```
UPDATE edinburghstreeswithastory SET longitude = split_part(location, ', ', 2)
```

### Edit the georeferencing information

Finally, click on the **Edit** button on the top righthand side of the screen, then choose the **Georeference** option.

![](images/carto5.tiff)

Click on the first box that says **Select column**, and choose **Longitude**. Do the same for the second box, and choose **Latitude** as the value. The window should now look like this:

![](images/carto10.tiff) 

Press **CONTINUE**.

After a bit of whirring, a popup window like the following should tell you that the georeferencing is complete:

![](images/carto11.tiff) 

## Create a map

Click **Visualize** in the top right 
to create your first map. On the next window, click **OK, CREATE MAP**. Then click **MAP VIEW**. You will have to use the zoom (+) to zoom into Edinburgh, and see where the tree locations have been placed.

You can change the background map by clicking **Change basemap** in the bottom left. Positron is a good default basemap, but there are many other options available. 

Click **Options** in the bottom left to select the map interaction options you want to provide to the visitors of your map, such as Zoom controls or a Fullscreen button.

The **MAP VIEW** also provides a toolbar on the right, where you'll recognize some of the same tools from the **DATA VIEW** screen.  Click **Wizards** in the toolbar to see a wide range of visualization options. These are all explained in the [CartoDB documentation](http://docs.cartodb.com/cartodb-editor.html#wizards).



