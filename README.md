# CodeAlpha_Web-Scraping-
pip install requests beautifulsoup4 pandas

import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "http://books.toscrape.com/catalogue/category/books_1/index.html"
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")

books = []
for book in soup.select("article.product_pod"):
    title = book.select_one("h3 a")["title"]
    # Correct example:
    price = book.select_one("p.price_color").text.strip()
    price = price.replace("Â", "").replace("£", "").strip()
    price = float(price)
    rating = book.select_one("p.star-rating")["class"][1]
    books.append({"title": title, "price":(price),"rating":rating})

df = pd.DataFrame(books)
print("Scraped data:")
print(df)

average_price = df["price"].mean()
books_by_rating = df["rating"].value_counts()
print(f"Average price of books: {average_price: .2f}")
print("Number of the books by rating:")
print(books_by_rating)

df.to_csv("books.csv", index=False)
print("Dataset successfully saved to books.csv")

df=pd.read_csv("books.csv")
df
