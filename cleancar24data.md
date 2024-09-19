import pandas as pd
import re

df = pd.read_csv('car_data.csv')

def clean_text(text):
    if pd.isna(text):
        return text
    return re.sub(r'\s+', ' ', str(text).strip())

for col in df.select_dtypes(include=['object']).columns:
    df[col] = df[col].apply(clean_text)

df['KM_Driven'] = df['KM_Driven'].str.replace(',', '').str.extract('(\d+)').astype(float)

df['Monthly_EMI'] = df['Monthly_EMI'].str.replace('₹', '').str.replace(',', '').astype(float)

df['Owner_Type'] = df['Owner_Type'].str.lower()

df['Fuel_Type'] = df['Fuel_Type'].str.lower()

df['Car_Transmission'] = df['Car_Transmission'].str.lower()

df.drop_duplicates(inplace=True)

df.reset_index(drop=True, inplace=True)

print(df.head())

print(df.info())
print(df.describe())

df.to_csv('cleaned_car_data.csv', index=False)
print("Cleaned data has been written to 'cleaned_car_data.csv'")
