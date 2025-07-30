# 🧹 Data Cleaning & Filtering

### ➤ Filter rows with multiple conditions
```python
df = df[(df['duration'] > 0) & (df['duration'] < 1440)]
```

### ➤ Calculate trip duration (in minutes)
```python
df['duration'] = (df['dropoff'] - df['pickup']).dt.total_seconds() / 60
```

### ➤ Remove zero/negative values to avoid `-inf`
```python
df = df[df['trip_distance'] > 0].copy()
```

### ➤ Handle SettingWithCopyWarning
```python
df = df.copy()  # before modifying columns
df.loc[:, 'col'] = ...
```

# 📊 EDA (Exploration)

### ➤ Count columns
```python
len(df.columns)
```

### ➤ Top 10 or bottom 10 rows
```python
df.nlargest(10, 'duration')
df.nsmallest(10, 'duration')
```

### ➤ Describe percentiles
```python
df['duration'].describe(percentiles=[0.99])
```

### ➤ Plot distribution
```python
sns.histplot(df['duration'], bins=100)
```

# 🧮 Feature Engineering

### ➤ Create new column safely
```python
df['fare_per_mile'] = df['fare_amount'].astype(float) / df['trip_distance'].astype(float)
```

# 🧠 One-Hot Encoding (pandas)

### ➤ One-hot encode multiple columns
```python
df['col1'] = df['col1'].astype(str)
df['col2'] = df['col2'].astype(str)
df_encoded = pd.get_dummies(df, columns=['col1', 'col2'], drop_first=True)
```

# 🧰 DictVectorizer (scikit-learn)

### ➤ Convert to list of dicts
```python
categorical = df[['col1', 'col2']].to_dict(orient='records')
```

### ➤ Fit & transform (sparse = memory-efficient)
```python
from sklearn.feature_extraction import DictVectorizer

dv = DictVectorizer(sparse=True)
X = dv.fit_transform(categorical)     # One-hot matrix
```

### ➤ Inspect feature matrix
```python
X.shape                   # (rows, columns)
dv.get_feature_names_out()  # feature names
```

# 🧱 Sparse vs Dense

| Type    | Memory | Use Case              |
|---------|--------|------------------------|
| Sparse  | Low    | One-hot encoding, large data |
| Dense   | High   | Small feature sets only |
