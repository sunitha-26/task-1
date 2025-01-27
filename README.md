import requests
from bs4 import BeautifulSoup
import json
import csv

def scrape_website(url, output_format='json', selectors=None):
    """
    Scrapes a website and stores the data in a structured format (JSON or CSV).

    :param url: The URL of the website to scrape.
    :param output_format: Output format: 'json' or 'csv'. Default is 'json'.
    :param selectors: A dictionary of CSS selectors to extract specific elements.
    :return: None
    """
    if selectors is None:
        selectors = {
            "headlines": "h1, h2, h3",
            "links": "a[href]",
            "paragraphs": "p"
        }

    try:
        response = requests.get(url)
        response.raise_for_status()
        soup = BeautifulSoup(response.text, 'html.parser')

        scraped_data = {}
        for key, selector in selectors.items():
            elements = soup.select(selector)
            scraped_data[key] = [
                element.get_text(strip=True) if key != 'links' else element['href']
                for element in elements
                if element
            ]

        if output_format.lower() == 'json':
            with open('scraped_data.json', 'w', encoding='utf-8') as json_file:
                json.dump(scraped_data, json_file, ensure_ascii=False, indent=4)
            print("Data saved to scraped_data.json")

        elif output_format.lower() == 'csv':
            with open('scraped_data.csv', 'w', newline='', encoding='utf-8') as csv_file:
                writer = csv.writer(csv_file)
                writer.writerow(["Category", "Content"])
                for key, values in scraped_data.items():
                    for value in values:
                        writer.writerow([key, value])
            print("Data saved to scraped_data.csv")

        else:
            print("Unsupported output format. Use 'json' or 'csv'.")

    except requests.exceptions.RequestException as e:
        print(f"Error fetching the URL: {e}")

    except Exception as e:
        print(f"An error occurred: {e}")

if _name_ == "_main_":
    target_url = input("Enter the URL to scrape: ")
    output_format = input("Enter output format (json/csv): ")
    print("Starting the scraping process...")

    # Default selectors can be modified based on specific website needs
    selectors = {
        "headlines": "h1, h2, h3",
        "links": "a[href]",
        "paragraphs": "p"
    }

    scrape_website(target_url, output_format, selectors)
