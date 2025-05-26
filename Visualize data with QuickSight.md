<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Visualize data with QuickSight

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-analytics-quicksight)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_6c7f7ef0)

---

## Introducing Today's Project!

In this project, I will demonstrate how to use Amazon QuickSight to analyze Netflix data and generate visualizations and insights! I'm doing this project to learn how to use cloud data services for data analysis.

### Tools and concepts

Services I used were QuickSight and S3. Key concepts I learnt include include manifest.json files, data visualization techniques (e.g. charts, filters) and how to perform a data refresh in QuickSight.

### Project reflection

This project took me approximately 2 hours including demo time! The most challenging part was when trying to refresh my data after uploading the new data QuickSight did't find my S3 bucket, what I realized was my Manifest.json wasn't pointer was missing proper refers to the bucket. It was most rewarding to be able to go fix that error also generating a PDF of my finished visualization.

After this project, I plan to work on Cloud Security with AWS IAM I will start this project on hopefully tomorrow!

---

## Upload project files into S3

S3 is used in this project to store two files, which are manifest.json (which tells QuickSight about the structure and format of the data we're uploading we're analyzing ) and netflix_titles.csv (which is the raw data that we're here to analyzing today).

I edited the manifest.json file by updaing the S3 URI that corresponds to our dataset's file location. It's important to edit this file because it's how Quicksight will find and analyze that data- if we didn't update this file, QuickSight wouldn't know the correct locationof the dataset. This will cause error down the line.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_3c3cd85a)

---

## Create QuickSight account

Creating a QuickSight account cost $0 as comes witha 30 day free trial! Make sure you UNCHECK an add-on in the sign up flow called Pixel-Perfect Reports so you don't get charged.

Creating an account took me about 5 ish minutes, including setting up my S3 bucket's permissions.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_f4ab4214)

---

## Download the Dataset

I connected the S3 bucket to QuickSight by visiting the Datasets page. There were SO many options for data sources we could connect to (databases and external tools/platforms like Saleforce), but I selected S3.

The manifest.json file was important in this step because it tells QuickSight how to read the data -in this project, it tells QuickSight that we're uploading a .CSV file (spreadsheet) and the delimiter (i.e commas) so that QuickSight knows how to break up the data for analysis. Otherwise, QuickSight might get confused.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_6f874996)

---

## My first visualization

To create visualizations on QuickSight, I simply have to click on dat field/tools e.g. release_year and QuickSight will automatically generate a graphic that best suits that type of data. You can also drag data labels into sections like "Group By" or "Y-axis" to determine how our graphic should treat the data.

The chart shown here is a breakdown of the release years of the content inside Netflix - i.e how many TV shows/movies were released on xyz you? You can see that there is total of 8800+ content pieces, and 2019 is the year with the most amount of content released.

I created this graph by simply clicking on the 'release_year' data label, and changing the automatically generated chart from a bar chart to a donut chart.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_aff3aad7)

---

## Using filters

Filters are useful for narrowing down our data to subset that we want to focus on, and in this case, we could use filters to focus on the specific categories that I want to analyze! We also used filters to only look at content with a relase date from 2015 and beyond.

This visualization is a breakdown of TV shows/movies that belong in one of three categories - action and adventure, tv comdies and thrillers, Here I added a filter based on the 'listed on' data label i.e only theses three catgories could pass the filter. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_c32248c5)

---

## Setting up a dashboard

As a finishing touch, I updated the titles of the charts in my dashboard so that they are easily readable. The default names would simply mention the data labels e.g. relased_ year, but the new titles communicate the purpose of the chart clearly e.g. # of Movies vs TV Shows by Release Years.

Did you know you could export your dashboard as PDFs too? I did this by selecting Publish and then "Generate PDF" on the top right handcorner of my QuickSight Analysis screen.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_6c7f7ef0)

---

## Refreshing source data

In this project's extension, I downloaded fresh data that's different from my original dataset because it had rows and rows of empty Country data. Analysing incomplete data brings the risk of generating inaccurate insights, which leads to the wrong busniess decisions that cost the company time, effort and money.

Once I downloaded new data, I had to update my S3 bucket because it is still storing the previous version of the data (i.e. the one with country data missing). I also uploaded a new manifest.json file that points to our updated dataset's file name. This makes sure that QuickSight is now pulling in data from the upload dataset, and not the verison with missing data.

I initally couldn't see my updated data in QuickSight, so I had to visit the dataset in the Datasets page in quickSight, and perform a full refresh of our data!

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-analytics-quicksight_86415f4e3)

---

---
