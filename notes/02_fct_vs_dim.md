### 📊 2. Fact Tables vs Dimension Tables

#### ✅ Fact Table

* Stores measurable **events or transactions**
* Has **foreign keys** to dimensions
* Contains **quantitative metrics**

**Example: `fact_sales`**

| sale\_id | customer\_id | product\_id | quantity | amount |
| -------- | ------------ | ----------- | -------- | ------ |
| 1        | 10           | 200         | 2        | 100.0  |

#### ✅ Dimension Table

* Describes entities: who, what, where
* Textual attributes used for slicing, filtering

**Example: `dim_product`**

| product\_id | name    | category |
| ----------- | ------- | -------- |
| 200         | T-Shirt | Apparel  |

#### 🌟 Star Schema

* Central fact table with surrounding denormalized dimension tables
* Simple, fast queries

#### ❄️ Snowflake Schema

* Dimensions are normalized into sub-dimensions
* Less redundancy, more joins, slower
