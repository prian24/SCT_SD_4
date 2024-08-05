# SCT_SD_4
This Python program scrapes product information—such as names, prices, and ratings—from an e-commerce website and saves it to a CSV file. It uses the requests library to retrieve webpage content and BeautifulSoup to parse HTML and extract details. The data is then written to a CSV using the csv library. 
import requests
from bs4 import BeautifulSoup
import csv

# Function to extract product information from a webpage
def extract_product_info(url):
    # Send a request to the webpage
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"}
    response = requests.get(url, headers=headers)

    # Parse the webpage content
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find all product items (This part should be adjusted according to the website's structure)
    products = soup.find_all('div', class_='product-item')

    product_list = []

    for product in products:
        # Extract product details (names, prices, ratings)
        name = product.find('h2', class_='product-title').get_text(strip=True)
        price = product.find('span', class_='price').get_text(strip=True)
        rating = product.find('div', class_='ratings').get_text(strip=True)

        # Append the product details to the list
        product_list.append([name, price, rating])

    return product_list

# Function to save product information to a CSV file
def save_to_csv(products, filename='products.csv'):
    # Define the CSV header
    header = ['Name', 'Price', 'Rating']

    # Write to a CSV file
    with open(filename, mode='w', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerow(header)  # Write the header
        writer.writerows(products)  # Write the product data

# URL of the e-commerce page to scrape (Change this to the actual page you want to scrape)
url = 'https://www.example.com/products'

# Extract product information from the page
product_info = extract_product_info(url)

# Save the extracted product information to a CSV file
save_to_csv(product_info)

print(f"Product information has been saved to 'products.csv'.")
