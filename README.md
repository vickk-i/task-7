

---

## 🛒 Sales Data Analysis using SQLite & Python

This notebook demonstrates a complete mini-project where we use **SQLite** for managing sales data and **Python** (via **pandas** and **matplotlib**) to query, analyze, and visualize revenue by product.

---

### **1. 📦 Importing Required Libraries**

```python
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt
```

We start by importing essential libraries:
- `sqlite3` for working with an embedded SQL database
- `pandas` for data manipulation
- `matplotlib.pyplot` for plotting graphs

---

### **2. 🛠️ Database Setup and Data Insertion**

```python
# connect to SQLite (creating sales_data.db )
conn = sqlite3.connect("sales_data.db")
cursor = conn.cursor()

#creating sales table
cursor.execute("""
CREATE TABLE IF NOT EXISTS sales(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    product TEXT,
    quantity INTEGER,
    price REAL
)
""")
```

✅ A SQLite database named **`sales_data.db`** is created. A `sales` table is defined to store product name, quantity, and price details.

```python
#Inserting values
sample_data = [
    ('Apple',10,0.5),
    ('Banana',5,0.3),
    ('Orange',8,0.4),
    ('Apple',7,0.5),
    ('Banana',10,0.3)
]

cursor.executemany("INSERT INTO sales (product,quantity,price) VALUES (?,?,?)", sample_data)
conn.commit()
```

🧾 Sample sales data is inserted into the table. This dataset represents product-wise quantity sold and their respective prices.

---

### **3. 🧮 Revenue Calculation Using SQL**

```python
query = """
SELECT product,
      SUM(quantity) AS total_qty,
      SUM(quantity * price) AS revenue
FROM sales
GROUP BY product
"""
```

🔎 The SQL query aggregates:
- **Total quantity sold** per product
- **Total revenue** per product (`quantity * price`)

```python
df = pd.read_sql_query(query,conn)
print(df)
```

📊 The query results are loaded into a pandas DataFrame for further processing and visualization.

---

### **4. 📈 Visualizing Revenue by Product**

```python
df.plot(kind ='bar',x='product',y='revenue',title='Revenue by Product',legend=False)
plt.xlabel('Product')
plt.ylabel('Revenue')
plt.tight_layout()
plt.show()
```

🎨 A **bar chart** is generated showing revenue distribution by product. This helps in quickly identifying the top-performing items.

---

### **5. 💾 Saving the Plot**

```python
plt.figure()
df.plot(kind='bar',x='product',y='revenue',title='Revenue by Product',legend=False)
plt.xlabel('Product')
plt.ylabel('Revenue')
plt.tight_layout()
plt.savefig("sales_chart.png")
```

📤 The plot is saved as a PNG image (`sales_chart.png`) for sharing, reporting, or documentation purposes.

---

### **6. 🔐 Closing the Database Connection**

```python
conn.close()
```

✅ The database connection is gracefully closed after all operations are complete.

---

### ✅ Final Summary

- Built a local database and populated it with product sales data.
- Used SQL queries to calculate total sales and revenue per item.
- Visualized the insights using bar charts.
- Exported the chart as an image for later use.

> This notebook showcases how SQL and Python together can deliver quick and effective business insights, even from small datasets.

---
