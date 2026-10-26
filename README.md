# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output

```
import pandas as pd
import numpy as np

df = pd.read_csv("Data_set.csv")

print("\nOriginal Data:\n", df.head(10))
print("\nMissing values:\n", df.isnull().sum())

clean = df.copy()

for col in clean.select_dtypes(include="number").columns:
    clean[col] = clean[col].fillna(clean[col].median())

for col in clean.select_dtypes(include="object").columns:
    if not clean[col].mode().empty:
        clean[col] = clean[col].fillna(clean[col].mode()[0])

print("\nMissing values after cleaning:\n", clean.isnull().sum())
print("\nCleaned Data:\n", clean.head(10))

clean.to_csv("Data_set_cleaned.csv", index=False)
print("\nSaved Data_set_cleaned.csv")
```

<img width="1102" height="765" alt="image" src="https://github.com/user-attachments/assets/1d7f6846-a0e5-41b7-a37f-f31528430019" />
<img width="972" height="775" alt="image" src="https://github.com/user-attachments/assets/b3ca0fbf-c82f-4392-adf1-8eec14ca8a91" />
<img width="1060" height="532" alt="image" src="https://github.com/user-attachments/assets/e4e235d0-06b5-4022-a81b-dab443c729ea" />


```
import pandas as pd
import numpy as np

df = pd.read_csv("SAMPLEIDS.csv")

print("\nOriginal Data:\n", df.head(10))
print("\nDuplicate rows:", df.duplicated().sum())

clean = df.drop_duplicates().copy()
for col in clean.select_dtypes(include="object").columns:
    clean[col] = clean[col].str.strip()
    print("\nUnique values in", col)
    print(clean[col].unique())

print("\nDuplicate rows after removal:", clean.duplicated().sum())
print("\nCleaned Data:\n", clean.head(10))

clean.to_csv("SAMPLEIDS_cleaned.csv", index=False)
print("\nSaved SAMPLEIDS_cleaned.csv")
```


<img width="1137" height="752" alt="image" src="https://github.com/user-attachments/assets/15cfce6b-6c3f-485e-b723-7135f32a7104" />
<img width="1067" height="740" alt="image" src="https://github.com/user-attachments/assets/668f5554-6d47-470a-8d35-3ef2dd3fda94" />


```
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

df = pd.read_csv("heights.csv")

print("\nOriginal Data:\n", df.head(10))

for col in df.select_dtypes(include="number").columns:

    values = df[col].dropna()

    if values.empty:
        continue

    q1 = values.quantile(0.25)
    q3 = values.quantile(0.75)
    iqr = q3 - q1

    lower = q1 - 1.5 * iqr
    upper = q3 + 1.5 * iqr

    outliers = values[(values < lower) | (values > upper)]

    print("\nIQR results for", col)
    print("Lower limit:", lower)
    print("Upper limit:", upper)
    print("Unusual measurements:\n", outliers)

for col in df.select_dtypes(include="number").columns:
    plt.figure()
    plt.boxplot(df[col].dropna())
    plt.title("Unusual measurements in " + col)
    plt.ylabel(col)
    plt.show()
```


<img width="522" height="446" alt="image" src="https://github.com/user-attachments/assets/29a06f0f-8cb9-4c9e-8c4d-6b6f5520198f" />
<img width="791" height="561" alt="image" src="https://github.com/user-attachments/assets/af966a08-da6b-4359-b37d-5040dbd8e8cf" />

```
import pandas as pd
import numpy as np
from scipy import stats
df = pd.read_csv("iris.csv")

print("\nOriginal Data:\n", df.head(10))


for col in df.select_dtypes(include="number").columns:

    values = df[col].dropna()

    if values.empty:
        continue

    q1 = values.quantile(0.25)
    q3 = values.quantile(0.75)
    iqr = q3 - q1

    lower = q1 - 1.5 * iqr
    upper = q3 + 1.5 * iqr

    outliers = values[(values < lower) | (values > upper)]

    print("\nIQR outliers in", col)
    print(outliers if not outliers.empty else "None")

# STEP 3: Z-SCORE OUTLIER DETECTION
for col in df.select_dtypes(include="number").columns:

    values = df[col].dropna()

    if len(values) < 2:
        continue

    z = np.abs(stats.zscore(values))
    outliers = values[z > 3]

    print("\nZ-score outliers in", col)
    print(outliers if not outliers.empty else "None")
```


<img width="712" height="762" alt="image" src="https://github.com/user-attachments/assets/c11077a5-ae1d-4a53-9bfe-ac431008171d" />

```
import pandas as pd
import numpy as np
from scipy import stats
df = pd.read_csv("Loan_data.csv")

print("\nOriginal Data:\n", df.head(10))


for col in df.select_dtypes(include="number").columns:

    values = df[col].dropna()

    if values.empty:
        continue

    q1 = values.quantile(0.25)
    q3 = values.quantile(0.75)
    iqr = q3 - q1

    lower = q1 - 1.5 * iqr
    upper = q3 + 1.5 * iqr

    outliers = values[(values < lower) | (values > upper)]

    print("\nIQR outliers in", col)
    print(outliers if not outliers.empty else "None")

# STEP 3: Z-SCORE OUTLIER DETECTION
for col in df.select_dtypes(include="number").columns:

    values = df[col].dropna()

    if len(values) < 2:
        continue

    z = np.abs(stats.zscore(values))
    outliers = values[z > 3]

    print("\nZ-score outliers in", col)
    print(outliers if not outliers.empty else "None")
```


<img width="1065" height="765" alt="image" src="https://github.com/user-attachments/assets/fbef806a-d572-4cd0-9947-01ae42a13289" />
<img width="1005" height="757" alt="image" src="https://github.com/user-attachments/assets/d2eb54d8-83d9-4fb1-89e1-c1baf218d692" />
<img width="742" height="760" alt="image" src="https://github.com/user-attachments/assets/c8ca25c2-31f1-46b3-ad30-18fe065c918f" />
<img width="642" height="640" alt="image" src="https://github.com/user-attachments/assets/448a41fb-11d1-4573-a2a8-d9220ef79305" />




# Result

The given data has been successfully read, cleaned by handling duplicates and missing values, and saved to a new file named cleaned_data.csv
