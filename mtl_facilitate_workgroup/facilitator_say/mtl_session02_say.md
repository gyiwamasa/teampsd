---
title: "MTL Session 02 - Facilitator Say"
author: "Team PSD"
date: "September 2018"
output: 
  github_document: default
  html_document: default
  pdf_document: default
  word_document: default
  ioslides_presentation: default
  slidy_presentation: default
  powerpoint_presentation: default
---

<img src = "https://github.com/lzim/teampsd/blob/master/resources/logos/mtl_live_sq_sm.png"
     height = "175" width = "290">  

# [MTL Live Session 02](https://github.com/lzim/teampsd/blob/master/mtl_facilitate_workgroup/mtl_live_guide/mtl_live_session02_see.Rmd "MTL Live Session 02")

# Today we're modeling to learn how to check our patient data and team trends.
Hello! I'm __________ and I'm __________ [Co-facilitators introduce themselves]. Today we're modeling to learn how to check our patient data and team trends.

## Done and Do (15 minutes)
<!-- Do/Done Tables -->
| <img src = "https://raw.githubusercontent.com/lzim/teampsd/hexagon_icons/np_hexagon-check-mark_309690_003F72.png" height = "80" width = "80"> **Done** | <img src = "https://raw.githubusercontent.com/lzim/teampsd/hexagon_icons/np_synchronize_778914_003F72.png" height = "90" width = "90"> **Do** |
| --- | --- | 
| [<img src = "https://raw.githubusercontent.com/lzim/teampsd/master/resources/logos/mtl_how_data_sm.png" height = "75" width = "110">](http://mtl.how/data) We identified our Team Vision, selected our Team Lead and set a standing team meeting time. We logged in to mtl.how/data to look at the splash page.| [<img src = "https://raw.githubusercontent.com/lzim/teampsd/master/resources/logos/mtl_how_data_sm.png" height = "75" width = "110">](http://mtl.how/data) We will select and review Team Data for _MTL_. |  

# Done
In MTL Session 1, we worked together and accomplished three important tasks.  

1.	Our team vision was developed  

2.	The Team Lead (or Co-Team Leads) was identified.  

+ Thank you ______________ (INSERT NAME) for taking on the role of Team Lead. *NOTE: If co-leads were selected, acknowledge both individuals.*  

3.	We selected the standing meeting time for this team which is _______________ (INSERT DAY AND TIME).	 

# Do
+ In the post-session email, you were asked to log into the data User Interface or data UI at mtl.how/data.  That is our purpose for today -- to explore your team’s data in the data UI found at mtl.how/data.  

<!-- Learning Objectives Icon --> 
<img src = "https://github.com/lzim/teampsd/blob/master/resources/icons/we_decided_learning_objectives.png" height = "90" width = "90" style ="display: inline-block"/> 

## Learning Objectives

1. Describe the decisions your team made in producing your team data table.  
2. Test out whether your expectations about team historical trends are displayed in the "viz" tabs.  
3. Apply your clinical expertise to identify new information about a team patient in the "data" tabs.  

# In-session Exercise (30 minutes)  

## *MTL* on BISL

+ The team data is stored on the VA’s corporate data warehouse site referred to as BISL [Business Intelligence Service Line]. We will refer to this as the BISL sharepoint splash page. 
+ The information in this data file is Protected Health Information (PHI). Please save your data file back to SharePoint or places that are appropriate for PHI. If attaching a data file to an email, encrypt the file.

### 1. Navigating to the data UI  

+ To get into the data UI we will go through a few steps.  

  1.	Type mtl.how/data in your web browser  
  2.	On the page titled “Select Your VISN”, click on your team’s VISN number  
  3.	And, on the page titled  “Select Your Facility”, click on the facility name for your team.  


### 2. Explore the information available in the BISL sharepoint splash page

+ Once you have selected your facility name, a web access version of Excel will open on your screen.  

+ Let's explore the BISL sharepoint splash page.  

  ++ How many scroll bars are there?  Where are they?  
     - You will see a scroll bar on the far right to move up and down to review the entire data UI screen.  
     - Another bar on the right side allows you to move up and down to see a list of variables that you can include in charts.  
     - Then, just like you have in Excel, there is a vertical and a horizontal scroll bar to move up and down rows and across left to right to see all the columns.  
     
  ++ Where are the tabs? What can you see by selecting the tabs? 
  
     - Let's review the tabs together. Across the bottom of the Excel chart starting left to right:  
        Control  - you can click on the cell next to the word "Station" and select your station number, then click "Get Clinic List"  
        ClinicSelection  - this is where you select the the clinics your team would like to include in your dataset  
        SPTransfers  - data about Suicide Prevention transfers  
        dataDiag - data about Diagnoses  
        dataHF - data about Health Factors  
        dataMeas - data about Measures or flag names  
        vizDiag - visuals or charts about Diagnoses  
        vizHF - visuals or charts about Health Factors  
        vizEnc - visuals or charts about Encounters, types of visits  
        visMeas - visuals or charts about Measures or flag names  
        CCParams - parameters for the *MTL* Care Coordination module
        MMParams - parameters for the *MTL* Medication Management module
        PSYParams - parameters for the *MTL* Psychotherapy module
        AggParams - parameters for the *MTL* Aggregate module
        
  ++ What filtering options are available? 
  
     - At present there are four tabs to use for filtering and sorting your team data.  
       “Diag” – This is diagnostic data based on visits in the team clinic dataset you selected.  
       “HF” – This is health factor data associated with those visits.  
       
     - When you filter to your clinic or division, you will see trends for the last two years.  

## Your Team Data Folder

### 3. Scroll to your team folder at the bottom of the page. Open the data_ui folder and open your team data ui in Excel.
+ Click on Control tab
+ Click on the cell next to the word "Station" and click on your station number.
   - If you have any issues with permissions, Team PSD can help you.
+ Click "Get Clinic List" and it will pull in clinics for your facility.

### 4. Go to the ClinicSelection tab. Use columns C-H to select the clinics that make up your team.
+ You can sort by Clinic Name, Division Name, Physical Location, Primary and Secondary Stopcode.
+ Or, if your team as designated provider clinics, you can select by provider name.
+ Note that this will pull all clinics used in the last two years (including de-activated clinics). You can see the de-activated clinics in column I.
+ Follow the instructions in **Box A2.** 
+ After filtering, you can double-click on clinics to add them to column A or you can highlight the clincis and click the gray "add all" button to add them to column A. 

#### 5. To view your team patient data and your team trends. You click on "Get-Patient-level Data." We will not do this in today's session.
- **When working with a team live, we will have already pulled a fresh data UI file to work with in a team's data folder.** 
- We will learn about the "Create Team Data Table for Sim UI" button in our next session.

### NOTE: It takes some time to run a query from team data UI to the VA Corporate Data Warehouse. 
- On average (depending on the size of your team) it may take 15 minutes or so for your team data UI to pull in fresh data. 
- It is important to note that Microsoft Excel will be unresponsive until the data UI has finished pulling in your data.


### 6. Click to view the "viz" tabs, which show team trends.
- There are team trends for diagnoses, encounters, health factor data (e.g., suicide plans, evidence-based practice templates), and measures from mental health assistant.
- *NOTE: Do we need to create sample chart for video*
- What stands out to you?
- What is most important to you to check out first?
- what is most surprising?

### 7. Click to view the "data" tabs, which show your team's individual patient information.
- Patients who have requested restricted access to their information have asterisks *******
- Patient information correponds to the same categories as the team trends: diagnoses, encounters, health factor data (e.g., suicide plans, evidence-based practice templates), and measures from mental health assistant.
- Providers can filter to find specific patients, or produce reports. 
- What **data** tab would you use to find out how many current patients on the team are engaged in a specific evidence-based psychotherapy? What column shows you the session number (EBP template) that the patient is on.
- What **viz** tab would you use to see what the most common service encounters or visits are?
- Are there services that have been increasing over time? Are there services that  have been descreasing over time?

## With the team trends (viz) and team patient (data) information in the data UI, your team can efficiently use team meetings to focus on the interrelated issues of care coordination and team quality improvement.

## Done and Do (15 minutes)
<!-- Do/Done Tables -->
| <img src = "https://raw.githubusercontent.com/lzim/teampsd/hexagon_icons/np_hexagon-check-mark_309690_003F72.png" height = "80" width = "80"> **Done** | <img src = "https://raw.githubusercontent.com/lzim/teampsd/hexagon_icons/np_synchronize_778914_003F72.png" height = "90" width = "90"> **Do** |
| --- | --- | 
| [<img src = "https://raw.githubusercontent.com/lzim/teampsd/master/resources/logos/mtl_how_data_sm.png" height = "75" width = "110">](http://mtl.how/data) We selected the clinics that make up our team for the Team Data. | [<img src = "https://raw.githubusercontent.com/lzim/teampsd/master/resources/logos/mtl_how_data_sm.png" height = "75" width = "110">](http://mtl.how/data) Review the HF, Diag, Enc and SP tabs in Team Data to find a patient (zoom in) and find a team trend (zoom out). | 


