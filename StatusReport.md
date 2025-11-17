Update on Tasks:

 So far, we have completed all of the required tasks up until Reproducibility and Transparency. We identified our data lifecycle, addressed ethical concerns, acquired the data, identified storage and organization methods, extracted and enriched the data, integrated the datasets together, assessed the data quality, and cleaned the integrated dataset. We are currently in the process of analyzing trends and creating visualizations that portray the integrated dataset. We are yet to create and implement a workflow, address reproducibility and transparency, and create the metadata and documentation. 

 Our project can be related to the USGS lifecycle model as it follows a linear process to collect, document, and publish our findings using the following progression: Plan, Acquire, Process, Preserve, Publish/Share. For the planning step, we have determined the scope of our project using the two datasets, where we will be observing data from the “Chicago Crimes - 2001 to Present” and “Violence Reduction - Victims of Homicides and Non-Fatal Shootings” datasets, and integrating them. We acquired the data by downloading both datasets from the City of Chicago website portal. We processed our data through cleaning techniques that remove null values and other inconsistencies to ensure that we can seamlessly merge the two datasets together. We will then preserve the data by saving curated versions with metadata in our git repository. We will finally publish our results in the form of the final project, showcasing our findings along with our final integrated data.
 
 To address ethical concerns in our project, we first observed whether our project follows the four ethical principles described in the Menlo Report and Belmont Report: Respect for Persons, Beneficence, Justice, and Respect for Law & Public Interest. Our data was collected by the City of Chicago and released in a form that doesn’t reveal any individuals’ identities. Through our work, we do not intend to associate any identification with each data entry. The goal of our work is to inform people about public policy and safety using quantitative analysis. Our work is not intended to  be used for malicious purposes, such as contributing to societal inequities and stigmas. Our analysis treats all communities equally without utilizing harmful labels or comparisons that perpetuate biases. We follow the City of Chicago’s Open Data Terms of Use and provide sources of our data. We also address privacy and confidentiality by observing the measures the City of Chicago website already has, as well as how we intend to protect individual data. The City of Chicago datasets omit any names or IDS, and only publish block-level addresses. We further preserve confidentiality by aggregating the data together and never linking information to external identifying sources. The City of Chicago datasets are distributed under the Open Data Commons/ CC-BY style licenses, which require attribution, non-endorsement, and transparency. We address this by citing our sources and only using the derived data for educational, non-commercial purposes.

  We completed the data collection script (acquire.ipynb). This script uses the requests library to programmatically acquire the two datasets,
"Crimes - 2001 to Present" and "Violence Reduction - Victims of Homicides and Non-Fatal Shootings." It automatically creates the ‘data/raw/’ directory as part of our project's storage structure. The result of this step is that the two raw, untouched CSV files have been successfully downloaded and saved locally, making them available for the next stage of the pipeline.

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

  For the extraction and enrichment step (M6), we completed it by creating the clean.ipynb notebook. The "extraction" phase involved loading the raw CSVs from the ‘data/raw/’ folder into the pandas DataFrames. The "enrichment" phase then created new columns that added value and supported our analysis. The key enrichments we added were extracting ‘year’, ‘month’, ‘day_of_week’, and ‘time_of_day’ from the date column, mapping the ‘iucr’ codes to a human-readable ‘crime_category’ (e.g., 'HOMICIDE', 'ROBBERY'), and generalizing the specific ‘age’ column into a binned ‘age_group’ (e.g., '18-25') to support our ethical handling (M2) requirement. The result of this step is the creation of two new files, ‘crimes_clean.csv’ and ‘victims_clean.csv’, which are saved in the ‘data/processed/’ folder. These files are now clean, standardized, and enriched for integration.

  For the data integration section (M7 & M8), we completed it by creating the integration.ipynb notebook. This script loads the two clean datasets from the ‘data/processed/’ folder. It then performs a left merge using pd.merge, with the victims dataset as the left table to ensure all 62,812 victim records are preserved. The case_number was used as the merge key, and suffixes (_victim, _incident) were added to differentiate overlapping columns. This step produced our first significant results and key findings for our report. The first key finding was that the final merged file has 64,536 rows, which is more than the original victims file. This is a data quality finding that shows some ‘case_numbers’ in the 'Crimes' file are duplicates, matching a single victim record multiple times. The second key finding was that the post-merge check found 13,042 records where the ‘date_victim’ did not match the ‘date_incident'. The third key finding was that the post-merge check also identified 2,473 records where the victim's IUCR code did not match the incident's IUCR code. The final, merged dataset, merged_victims_and_crimes.csv, has been saved to the ‘data/final/’ folder and is now ready for our final analysis.

  To assess data quality, we looked at completeness, data types, and consistency. While assessing completeness, we observed how many records are missing critical keys such as case_number or incident_id using df[‘case_number’].isnull().sum(). The missing information prevents us from linking the two datasets together, keeping this issue in mind before cleaning. When looking at the data types, columns like age and year held placeholders like “unknown”. We also looked at syntactic inconsistencies such as variations in sex and race. “M”, “MALE”, “F”, and “FEMALE” were all possible entries to describe gender, which is assigned on a binary basis. To clean the data, we used a script that standardizes, corrects, and prepares the datasets for integration and analysis. We dropped unusable rows where the key fields are missing. For example, we used df.dropna(subset = [‘case_number’]) to drop the entries with missing case numbers. For categorical fields like sex, we consolidated the missing or ambiguous entries into “unknown/other” using df[‘sex’] = df[‘sex’].map(sex_map).fillna(“UNKNOWN/OTHER”). We also transformed the age and date columns to fit proper numeric and datetime formats to ensure consistency and accuracy in analyzing information over time. We used script such as df['age'] = pd.to_numeric(df['age'].replace('UNKNOWN', pd.NA), errors='coerce') and df['date'] = pd.to_datetime(df['date'], errors='coerce'). We also fix inconsistent column naming conventions using df.columns = df.columns.str.lower().str.replace(' ', '_') to prevent errors when merging the two datasets. To replace inconsistent categorical values so that entries are accurately represented, we applied mapping dictionaries. We cleaned up extra spaces to prevent mismatches between the two datasets using df['case_number'] = df['case_number'].str.strip().


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

For our project plan, we didn’t have too many changes to our project plan that we identified. The main changes we made were pushing the draft of our workflow and reproducibility to phase 2 of the timeline we created in the project plan. This gave us more time to work with the data itself and start to highlight answers to the research questions we posed in our project plan. This was crucial in making sure that our code was accurate in achieving what we wanted, so we could properly find the answers to each research question. A positive change we made to our project plan is that we shifted the deadline for the workflow and reproducibility. We finished 95% of our scripts and only need to conduct more analysis in our analysis.ipynb file, which is far ahead of our planned schedule in the timeline of our project plan.

Regarding changes we made to our project plan based on feedback received from Milestone 2, we focused on updating the datasets paragraph so that it highlights licensing and terms of use regulations. The original paragraph was updated to include a new, second paragraph that specifically addresses the datasets' licenses and terms of use, which were missing in the original draft. This new section clarifies that while the datasets are "public," this does not mean they are without restrictions. It explains that, as "Non-Federal" data on the data.gov catalog, they do not have a standard license. Instead, they are governed by the terms set by the publisher, which, in the case of our two datasets, is the City of Chicago. The paragraph then specifies these terms, which were located on the city's own data portal. The portal highlights the terms being a disclaimer regarding data accuracy and a strict prohibition on any commercial use of the data. This change moves beyond a simple "public" label and provides the specific, documented terms of use as required.


Rakshita Kunde Contributions:

So far, I have built a 4-step data processing pipeline ("acquire", "clean", "integrate", "analyze") to process two Chicago crime datasets. The project uses a structured filesystem ("data/raw", "processed", "final", "results") to manage the data lifecycle and ensure reproducibility. The "acquire.ipynb" script uses "requests" to download the raw data from the URLs. 

The "clean.ipynb" script then standardizes, cleans, and transforms the data. This includes fixing column names, dropping rows with missing "case_number" keys, and correcting "date" and "age" data types. A key part of the cleaning step was "Ethical Handling (M2)". I anonymized the data by dropping high-risk identifiers (like "age", "block", and "latitude") and generalizing them into safer fields like "age_group". This script also “Enriched the data (M6)" by adding new "time_of_day" and "crime_category" columns, saving the clean files to "data/processed/". 

"integration.ipynb" performs a "left" merge on "case_number" to join the two clean datasets. This step revealed our first major findings: row duplication from the merge ("62k to 64k rows"), proving "case_number" is not unique in the "Crimes" file; "13,042 date mismatches" between the two files; and "2,473 IUCR code mismatches". The final merged file was saved to "data/final/".

"main_analysis.ipynb" loads the final merged file and runs three analysis functions, saving the results to text files. It successfully calculated the data update lag ("Q1"), reported on the Top 20 IUCR mismatches ("Q7"), and demonstrated an ethical analysis ("M2") using the new generalized data. 

I also updated our Project Plan with the feedback we received from Milestone 2 to ensure we met all the criteria and requirements. In addition, I identified the changes to the Project Plan we made so we can best allocate the remainder of our time to finish the rest of our project effectively.


Akanksha Kashyap Contributions:

For this project, I related the database lifecycle to the two chosen datasets. I also looked through the metadata to identify any ethical concerns and how we should address them in the context of this project. I also outlined how we store our data and the structure of our organization. I assessed our data quality after integration and observed what needs to be modified for the datasets to integrate seamlessly with each other. I also described the data cleaning techniques we used to read both datasets into the integrated dataset easily. 
