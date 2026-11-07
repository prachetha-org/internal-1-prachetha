# pandas_practice.py
import pandas as pd
import numpy as np


# 1. Create a DataFrame and calculate Product = A * B
df1 = pd.DataFrame(
    np.random.randint(1, 100, size=(5, 3)),
    columns=["A", "B", "C"]
)
df1["Product"] = df1["A"] * df1["B"]
print("DataFrame with Product column:")
print(df1)


# 2. Row-wise and column-wise sums
df2 = pd.DataFrame(
    np.random.randint(1, 100, size=(4, 3)),
    columns=["A", "B", "C"]
)

row_sum = df2.sum(axis=1)
col_sum = df2.sum(axis=0)

print("\nRow-wise Sum:")
print(row_sum)

print("\nColumn-wise Sum:")
print(col_sum)


# 3. Fill missing values with column means
df3 = pd.DataFrame(
    np.random.randint(1, 100, size=(5, 3)),
    columns=["A", "B", "C"]
).astype(float)

df3.iloc[0, 1] = np.nan
df3.iloc[2, 0] = np.nan
df3.iloc[4, 2] = np.nan

df3_filled = df3.fillna(df3.mean(numeric_only=True))

print("\nDataFrame after filling missing values with column means:")
print(df3_filled)


# 4. Drop rows containing missing values
df4 = pd.DataFrame(
    np.random.randint(1, 100, size=(6, 4)),
    columns=["A", "B", "C", "D"]
).astype(float)

df4.iloc[1, 2] = np.nan
df4.iloc[5, 0] = np.nan

df4_dropped = df4.dropna()

print("\nDataFrame after dropping rows with missing values:")
print(df4_dropped)


# 5. Group by category and calculate sum and mean
df5 = pd.DataFrame({
    "Category": np.random.choice(["A", "B", "C"], size=10),
    "Value": np.random.randint(1, 100, size=10)
})

agg_df = df5.groupby("Category")["Value"].agg(["sum", "mean"])

print("\nGrouped sum and mean by Category:")
print(agg_df)


# 6. Total sales by category
df6 = pd.DataFrame({
    "Product": ["Item1", "Item2", "Item3", "Item4", "Item5", "Item6"],
    "Category": np.random.choice(["Electronics", "Clothing", "Home"], size=6),
    "Sales": np.random.randint(100, 1000, size=6)
})

total_sales = df6.groupby("Category")["Sales"].sum().reset_index()

print("\nTotal sales by Category:")
print(total_sales)


# 7. Inner join two DataFrames on ID
df7a = pd.DataFrame({
    "ID": [1, 2, 3, 4],
    "Name": ["Alice", "Bob", "Charlie", "David"]
})

df7b = pd.DataFrame({
    "ID": [3, 4, 5, 6],
    "Score": [85, 92, 78, 88]
})

merged_df = pd.merge(df7a, df7b, on="ID", how="inner")

print("\nInner-joined DataFrame:")
print(merged_df)


# 8. MultiIndex DataFrame and selection
arrays = [["A", "A", "B", "B"], ["X", "Y", "X", "Y"]]
index = pd.MultiIndex.from_arrays(
    arrays,
    names=["Level1", "Level2"]
)

df8 = pd.DataFrame(
    np.random.randint(1, 100, size=(4, 2)),
    index=index,
    columns=["Val1", "Val2"]
)

print("\nLevel 'A' Data:")
print(df8.loc["A"])

print("\nSpecific Index ('B', 'Y'):")
print(df8.loc[("B", "Y")])


# 9. MultiIndex created from product and grouped sums
index9 = pd.MultiIndex.from_product(
    [["Tech", "Auto"], ["Sales", "Marketing"]],
    names=["Category", "SubCategory"]
)

df9 = pd.DataFrame(
    {"Employees": np.random.randint(10, 50, size=4)},
    index=index9
)

sum_by_category = df9.groupby("Category")["Employees"].sum()
sum_by_subcategory = df9.groupby("SubCategory")["Employees"].sum()

print("\nSum by Category:")
print(sum_by_category)

print("\nSum by SubCategory:")
print(sum_by_subcategory)


# 10. Apply a function to double every value
df10 = pd.DataFrame(
    np.random.randint(1, 10, size=(5, 3)),
    columns=["A", "B", "C"]
)

df10_doubled = df10.apply(lambda col: col * 2)

print("\nDataFrame with all values doubled:")
print(df10_doubled)
