# Web-scraper
This Python script is designed to perform web scraping with multithreading, focusing on gathering and analyzing website content for potential malicious links. It leverages the requests library to fetch web pages, BeautifulSoup from the bs4 module for HTML parsing, and Python's threading module to enable concurrent scraping of multiple websites.


import threading
import requests
from bs4 import BeautifulSoup
import time

# List of websites to scrape
websites = [Your_website_address]

# Function to scrape a website and check for potential malicious content
def scrape_website(url):
    try:
        print(f"Scraping {url}")
        response = requests.get(url)
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Extract all links from the page
        links = [a['href'] for a in soup.find_all('a', href=True)]
        print(f"Found {len(links)} links on {url}")
        
        # Check for any suspicious links or content (basic check for phishing URLs)
        for link in links:
            if 'login' in link or 'password' in link:
                print(f"Potentially malicious link found on {url}: {link}")
                
        time.sleep(1)
        
    except Exception as e:
        print(f"Error scraping {url}: {e}")

# Multithreading function
def multithread_scraping():
    threads = []
    for url in websites:
        thread = threading.Thread(target=scrape_website, args=(url,))
        threads.append(thread)
        thread.start()
    
    for thread in threads:
        thread.join()

# Main function
def main():
    print("Starting web scraping with multithreading...")
    start_time = time.time()
    
    multithread_scraping()
    
    end_time = time.time()
    print(f"Scraping completed in {end_time - start_time} seconds")

if __name__ == "__main__":
    main()
