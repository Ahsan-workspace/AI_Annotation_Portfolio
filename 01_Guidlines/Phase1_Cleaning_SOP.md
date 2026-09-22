# Data Cleaning using python(pandas)
# 1. Importing pandas and loading dataset csv file
import pandas as pd
df=pd.read_csv("Phase1_Enterprise_Chaotic_Data.csv")

# 2. Checking Dataset condition
   1. Checking number of rows 
print("Row Count: " ,len(df))
   2. Viewing info of dataset like column names , datatype etc 
df.info()
   3.  Checking number of duplications
duplicates=df.duplicated().sum()
print("Total duplicates :",duplicates)

   4. Checking corrupted names
names = df['Full_Name'].astype(str).str.contains(r'[#!]', regex=True).sum()
print(f"Found Corrupted Names: {names}")

   5. Checking missing emails
missing_emails = df['Contact_Email'].isnull().sum()
print(f"Found Missing Emails: {missing_emails}")

   6. Checking phone numbers
messy_phones = df['Phone_Number'].astype(str).str.contains(r'\D', regex=True).sum()
print(f"Found Messy Phone Numbers: {messy_phones}")

   6. Checking misspelled subscription tiers
typo_tiers = df['Subscription_Tier'].isin(["basc", "PRO_tier", "Entrp"]).sum()
print(f"Found Category Typos: {typo_tiers}")

   7. Checking for text hiding inside the numeric spend column
corrupted_spend = df['Total_Spend_USD'].astype(str).str.contains(r'\$').sum()
print(f"Found Corrupted Numbers: {corrupted_spend}")

   8. Checking mixed date formats
mixed_dates = df['Signup_Date'].astype(str).str.contains(r'/').sum()
print(f"Found Inconsistent Dates: {mixed_dates}\n")
  # 3. Cleaning the Data
1. removing duplicates
df.drop_duplicates(inplace=True)
print("Fixed: Duplicates removed.")

2. fixing names
df['Full_Name'] = df['Full_Name'].str.replace(r'[#!]', '', regex=True).str.strip().str.title()
print("Fixed: Name formats standardized.")

3. fixing emails
df['Contact_Email'].fillna('No Email Provided', inplace=True)
print("Fixed: Missing emails imputed.")

4. fixing phone numbers
df['Phone_Number'] = df['Phone_Number'].astype(str).str.replace(r'\D', '', regex=True)
print("Fixed: Phone numbers stripped to pure digits.")

5. fixing subscription tiers
tier_mapping = {"basc": "Basic", "PRO_tier": "Pro", "Entrp": "Enterprise"}
df['Subscription_Tier'] = df['Subscription_Tier'].replace(tier_mapping)
print("Fixed: Subscription categories remapped.")

6. fixing numeric spend column
df['Total_Spend_USD'] = df['Total_Spend_USD'].astype(str).str.replace('$', '').astype(float)
print("Fixed: Currency symbols stripped and converted to float.")

7. fixing date format
df['Signup_Date'] = pd.to_datetime(df['Signup_Date'], format='mixed').dt.strftime('%Y-%m-%d')
print("Fixed: Dates normalized to YYYY-MM-DD.\n")
# 4. Verifying the Fixation
## Check if any exact row copies are left (Should be 0)
print(f"Remaining Duplicates: {df.duplicated().sum()}")

## Check if any emails are still blank (Should be 0)
print(f"Remaining Missing Emails: {df['Contact_Email'].isnull().sum()}")

## Check if any weird symbols (# or !) are left in names (Should be 0)
print(f"Remaining Corrupted Names: {df['Full_Name'].astype(str).str.contains(r'[#!]', regex=True).sum()}")

## Check if any bad subscription categories are left (Should be 0)
print(f"Remaining Category Typos: {df['Subscription_Tier'].isin(['basc', 'PRO_tier', 'Entrp']).sum()}")

## Check if any dollar signs are left in the money column (Should be 0)
print(f"Remaining Corrupted Numbers: {df['Total_Spend_USD'].astype(str).str.contains(r'\$').sum()}")

## Check if any dates still have slashes instead of dashes (Should be 0)
print(f"Remaining Inconsistent Dates: {df['Signup_Date'].astype(str).str.contains(r'/').sum()}")
## Check if any phone numbers still have letters, dashes, spaces, or parentheses (Should be 0)
print(f"Remaining Messy Phone Numbers: {df['Phone_Number'].astype(str).str.contains(r'\D', regex=True).sum()}\n")
 


