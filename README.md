# E-commerce Product Scraper

This project is a simple Python-based web scraper that extracts product
information from the **WebScraper.io test e-commerce website**.\
It collects **Category**, **Product Name**, **Price**, and **Star
Rating** from two product sections:

-   Laptops\
-   Touch Phones

The scraped data is saved into a CSV file.

------------------------------------------------------------------------

## 🚀 Features

-   Scrapes multiple categories
-   Extracts:
    -   Product name
    -   Price
    -   Star rating (filled stars count)
-   Saves all data into a CSV file
-   Uses `requests`, `BeautifulSoup`, and `csv`

------------------------------------------------------------------------

## 📂 Output Example (CSV)

  Category   Product Name      Price   Rating
  ---------- ----------------- ------- --------
  laptops    Lenovo ThinkPad   \$299   4
  touch      Samsung Galaxy    \$199   5

------------------------------------------------------------------------

## 🧩 Requirements

Install the necessary libraries:

``` bash
pip install requests beautifulsoup4
```

------------------------------------------------------------------------

## 🧠 How It Works

1.  Sends GET request to each category URL\
2.  Parses HTML using BeautifulSoup\
3.  Extracts product details\
4.  Saves everything into a CSV file

------------------------------------------------------------------------

## 📝 Code Used

``` python
import requests
from bs4 import BeautifulSoup
import csv

categories = [
    "https://webscraper.io/test-sites/e-commerce/allinone/computers/laptops",
    "https://webscraper.io/test-sites/e-commerce/allinone/phones/touch"
]

save_path = r"C:\Users\JK TECH COMPUTER\Desktop\ws portfolio\ecommerce_products.csv"

with open(save_path, "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(["Category", "Product Name", "Price", "Rating (Stars)"])  # headers

    for category_url in categories:
        response = requests.get(category_url)
        soup = BeautifulSoup(response.text, "html.parser")
        category = category_url.split("/")[-1]

        products = soup.find_all("div", class_="thumbnail")
        for product in products:
            # Name
            name_tag = product.find("a", class_="title")
            name = name_tag.text.strip() if name_tag else "N/A"

            # Price
            price_tag = product.find("h4", class_=lambda x: x and "price" in x)
            price = price_tag.text.strip() if price_tag else "N/A"

            # Rating (count filled stars)
            ratings_div = product.find("div", class_="ratings")
            if ratings_div:
                star_tags = ratings_div.find_all("span", class_="glyphicon glyphicon-star")
                rating = str(len(star_tags))  # number of filled stars
            else:
                rating = "N/A"

            writer.writerow([category, name, price, rating])

print("✅ Scraping completed! Data saved to:", save_path)
```

------------------------------------------------------------------------

## 📎 Download README

This file is generated automatically and can be uploaded directly to
GitHub.

------------------------------------------------------------------------

## 📜 License

This project is for educational and portfolio purposes.
