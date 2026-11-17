# Data

This folder holds no data files. The raw data is downloaded automatically by the Bronze notebook (`notebooks/01_bronze.ipynb`).

## Source

- Dataset: [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) on Kaggle
- Kaggle slug: `olistbr/brazilian-ecommerce`
- Content: about 100,000 orders placed at multiple marketplaces in Brazil (2016 to 2018)
- License: check the dataset page on Kaggle before reuse. Kaggle lists it as CC BY-NC-SA 4.0.

## Source files and Bronze tables

The Bronze notebook strips the `olist_` prefix and the `_dataset.csv` suffix from each file name. The result is the table name in the `bronze` schema.

| Kaggle file | Bronze table |
|---|---|
| `olist_customers_dataset.csv` | `bronze.customers` |
| `olist_geolocation_dataset.csv` | `bronze.geolocation` |
| `olist_order_items_dataset.csv` | `bronze.order_items` |
| `olist_order_payments_dataset.csv` | `bronze.order_payments` |
| `olist_order_reviews_dataset.csv` | `bronze.order_reviews` |
| `olist_orders_dataset.csv` | `bronze.orders` |
| `olist_products_dataset.csv` | `bronze.products` |
| `olist_sellers_dataset.csv` | `bronze.sellers` |
| `product_category_name_translation.csv` | `bronze.product_category_name_translation` |

## How the data gets in

1. The notebook reads a Kaggle API token from a Databricks secret scope (`kv-scope`, key `kaggle-api-token`). The scope is backed by Azure Key Vault. No token is stored in the code.
2. It downloads the dataset with the Kaggle API into a Unity Catalog Volume: `/Volumes/databrics_ecommerce/bronze/landing/<date>`.
3. It writes each CSV as a Delta table in `databrics_ecommerce.bronze`.

## Run it yourself

1. Create a Kaggle account and generate an API token.
2. Store the token in Azure Key Vault under the name `kaggle-api-token`.
3. Create a Databricks secret scope named `kv-scope` that points to that Key Vault.
4. Run `notebooks/01_bronze.ipynb`.

Do not commit the Kaggle token or any CSV file to this repository.
