Update on Tasks:

So far, we have completed all of the required tasks up until Reproducibility and Transparency. We identified our data lifecycle, addressed ethical concerns, acquired the data, identified storage and organization methods, extracted and enriched the data, integrated the datasets together, assessed the data quality, and cleaned the integrated dataset. We are currently in the process of analyzing trends and creating visualizations that portray the integrated dataset. We are yet to create and implement a workflow, address reproducibility and transparency, and create the metadata and documentation. 
Our project can be related to the USGS lifecycle model as it follows a linear process to collect, document, and publish our findings using the following progression: Plan, Acquire, Process, Preserve, Publish/Share. For the planning step, we have determined the scope of our project using the two datasets, where we will be observing data from the “Chicago Crimes - 2001 to Present” and “Violence Reduction - Victims of Homicides and Non-Fatal Shootings” datasets and integrating them together. We acquired the data by downloading both datasets off of the City of Chicago website portal. We processed our data through cleaning techniques that remove null values and other inconsistencies to ensure that we can seamlessly merge the two datasets together. We will then preserve the data by saving curated versions with metadata in our git repository. We will finally publish our results in the form of the final project, showcasing our findings along with our final integrated data.
 To address ethical concerns in our project, we first observed whether our project follows the four ethical principles described in the Menlo Report and Belmont Report: Respect for Persons, Beneficence, Justice, and Respect for Law & Public Interest, Our data was collected by the City of Chicago and released in a form that doesn’t reveal any individuals’ identity. Through our work, we do not intend on associating any identification to each data entry. The goal of our work is to inform people about public policy and safety using quantitative analysis. Our work is not intended to  be used for malicent purposes, such as contributing to societal inequities and stigmas. Our analysis treats all communities equally without utilizing harmful labels or comparisons that perpetuate biases. We follow the City of Chicago’s Open Data Terms of Use and provide sources of our data. We also address privacy and confidentiality by observing the measures the City of Chicago website already has, as well as how we intend on protecting individual data. The City of Chicago datasets omit any names or IDS, and only publish block-level addresses. We further preserve confidentiality by aggregating the data together and never linking information to external identifying sources. The City of Chicago datasets are distributed under the Open Data Commons/ CC-BY style licenses, which require attribution, non-endorsement, and transparency. We address this by citing our sources and only using the derived data for educational, non-commercial purposes.
Since we acquired both the Crimes and Violence Reduction datasets as .csv files, we chose to use a tabular model. These files showcase the data as tables with structured rows for each observation and columns for each feature. We are using a filesystem as our storage system since our Python scripts read and write files directly from directories located on disk. We use the following filesystem structure and naming conventions:

IS477 Project/

  acquire.ipynb
  

  clean.ipynb


  integrate.ipynb
  

  main_analysis.ipynb
  

  data/
  

   raw/
   

    crimes_2001_to_present.csv 
    

    violence_reduction_victims.csv	
    

  processed/
  

    crimes_clean.csv
    

    victims_clean.csv


  final/
  

    merged_victims_crimes.csv

  results/


   ethical_aggregate_analysis.txt
   

   iucr_inconsistency_analysis.txt
   

   lag_analysis.txt

To assess data quality, we looked at completeness, data types, and consistency. While assessing completeness we observed how many records are missing critical keys such as case_number or incident_id using df[‘case_number’].isnull().sum(). The missing information prevents us from linking the two datasets together, keeping this issue in mind before cleaning. When looking at the data types, columns like age and year held placeholders like “unknown”. We also looked at syntactic inconsistencies such as variations in sex and race. “M”, “MALE”, “F”, and “FEMALE” were all possible entries to describe gender, which is assigned on a binary basis. To clean the data, we used script that standardizes, corrects, and prepares the datasets for integration and analysis. We dropped unusable rows where the key fields are missing. For example, we used df.dropna(subset = [‘case_number’]) to drop the entries with missing case numbers. For categorical fields like sex, we consolidated the missing or ambiguous entries into “unknown/other” using df[‘sex’] = df[‘sex’].map(sex_map).fillna(“UNKNOWN/OTHER”). We also transformed the age and date columns to fit proper numeric and datetime formats to ensure consistency and accuracy in analyzing information over time. We used script such as df['age'] = pd.to_numeric(df['age'].replace('UNKNOWN', pd.NA), errors='coerce') and df['date'] = pd.to_datetime(df['date'], errors='coerce'). We also fix inconsistent column naming conventions using df.columns = df.columns.str.lower().str.replace(' ', '_') to prevent errors when merging the two datasets together. To replace inconsistent categorical values so that entries are accurately represented, we applied mapping dictionaries. We cleaned up extra spaces to prevent mismatches between the two datasets using df['case_number'] = df['case_number'].str.strip().


Updated Timeline:

Below is our updated timeline, outlining what has been completed and what is still in progress:

Phase 1 - Complete: 

Data Lifecycle and Ethical Handling, 10/14, Akanksha Kashyap, M1 & M2 - Complete

Data Collection/Acquisition Scripts, 10/14, Rakshita Kunde, M3 - Complete

Store & Organization Design, 10/14, Akanksha Kashyap, M4 & M5 - Complete

Extraction & Enrichment Scripts, 10/21, Rakshita Kunde, M6 - Complete

Data Quality & Cleaning, 10/21, Akanksha Kashyap, M9 & M10 - Complete

Data Integration Script, 10/21, Rakshita Kunde, M7 & M8 - Complete

Drafting Interim Status Report, 11/08, Rakshita Kunde & Akanksha Kashyap - Complete

Interim Status Report Submission, 11/16, Rakshita Kunde & Akanksha Kashyap, Interim Report & Git Release - Complete

Phase 2 - In Progress:

Workflow Automation Draft, 12/02, Akanksha Kashyap, M11 & M12

Reproducibility Setup Draft, 12/02, Rakshita Kunde, M13

Final Workflow Implementation, 12/06, Akanksha Kashyap, M11 & M12

Final Reproducibility Implementation, 12/06, Rakshita Kunde, M13

Metadata & Documentation Finalization, 12/06, Akanksha Kashyap, M15

Drafting Final Report, 12/09, Rakshita Kunde & Akanksha Kashyap

Final Project Submission, 12/10, Rakshita Kunde & Akanksha Kashyap, ALL REQUIREMENTS




Changes to Project Plan and Addressed Feedback from Milestone 2:


Rakshita Kunde Contributions:



Akanksha Kashyap Contributions:

For this project, I related the database lifecycle to the two chosen datasets. I also looked through the metadata to identify any ethical concerns and how we should address them in the context of this project. I also outlined how we store our data and the structure of our organization. I assessed our data quality after integration and observed what needs to be modified for the datasets to integrate seamlessly with each other. I also described the data cleaning techniques we used to read both datasets in the integrated dataset easily. 
