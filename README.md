import requests
from bs4 import BeautifulSoup

def scrape_books():
    # URL of the scraping sandbox website
    url = "http://toscrape.com"
    
    # Send an HTTP GET request to the website
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"}
    response = requests.get(url, headers=headers)
    
    # Check if the request was successful
    if response.status_code != 200:
        print(f"Failed to retrieve data. Status code: {response.status_code}")
        return

    # Parse the raw HTML content using Beautiful Soup and Python's built-in parser
    soup = BeautifulSoup(response.content, "html.parser")
    
    # Find all product containers (books) on the page
    books = soup.find_all("article", class_="product_pod")
    
    print(f"--- Successfully Scraped {len(books)} Books ---\n")
    
    # Loop through each book and extract the data
    for index, book in enumerate(books, start=1):
        # 1. Extract the book title (stored in the 'title' attribute of the anchor tag inside <h3>)
        title = book.h3.a["title"]
        
        # 2. Extract the price
        price = book.find("p", class_="price_color").text.strip()
        
        # 3. Extract availability (stock status)
        availability = book.find("p", class_="instock availability").text.strip()
        
        # Print the structured output
        print(f"Book #{index}")
        print(f"Title : {title}")
        print(f"Price : {price}")
        print(f"Status: {availability}")
        print("-" * 40)

if __name__ == "__main__":
    scrape_books()







    *****output*****
    --- Successfully Scraped 20 Books ---

Book #1
Title : A Light in the Attic
Price : £51.77
Status: In stock
----------------------------------------
Book #2
Title : Tipping the Velvet
Price : £53.74
Status: In stock
----------------------------------------
Book #3
Title : Soumission
Price : £50.10
Status: In stock
----------------------------------------
... [Output truncated for space] ...
----------------------------------------
Book #20
Title : It's Only the Himalayas
Price : £45.17
Status: In stock
----------------------------------------
