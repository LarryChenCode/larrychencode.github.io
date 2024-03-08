# Web Scraping Amazon Kindle Prices

![Web Scraping Project Banner](images/web-scraping/web-scraping.gif)

In this project, I demonstrate how to perform web scraping on Amazon to track the price of Kindle books. This task is accomplished using a variety of Python libraries to efficiently fetch data, parse HTML, and automate the monitoring process.

## Tools Used

The project leverages the following Python libraries:

- `BeautifulSoup` from `bs4` for parsing HTML and extracting the necessary data.
- `requests` for making HTTP requests to Amazon's website.
- `time` and `datetime` for handling timing and scheduling of the price checks.
- `smtplib` for sending email notifications.
- `csv` for reading from and writing to CSV files, facilitating data storage.
- `pandas` as an additional tool for data manipulation and analysis.

## Functionality

The core functionality of this project is encapsulated in a function designed to check the price of a specified Kindle book on Amazon periodically. If the price drops to or below a target price set by the user, the function automatically triggers an email notification to alert the user of this price change.

### How It Works

1. The specified book's URL is used to make a request to Amazon's website.
2. The page's HTML content is fetched and parsed using BeautifulSoup to extract the book's current price.
3. The extracted price is compared against the user's target price.
4. If the book's price is less than or equal to the target price, an email notification is sent to the user, alerting them of the price drop.
5. This process is scheduled to run at regular intervals using the `time` and `datetime` libraries, ensuring continuous monitoring.

### Code
```python
# Create check_price function and combine all of the above code into one function
def check_price():
    URL = 'https://www.amazon.com/Becoming-Data-Head-Understand-Statistics-ebook/dp/B092TSVZJ8/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr='
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}
    page = requests.get(URL, headers=headers)
    soup1 = BeautifulSoup(page.content, "html.parser")
    soup2 = BeautifulSoup(soup1.prettify(), "html.parser")
    title = soup2.find(id='productTitle').get_text()
    price = soup2.find(id='kindle-price').get_text()
    price = price.strip()[1:]
    title = title.strip()
    today = datetime.date.today()
    header = ['Title', 'Price', 'Date']
    data = [title, price, today]

    with open('KindlePrice.csv', 'a+', newline='', encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)

    if(price < 20):
      send_mail()

# Create a send_mail function
def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com',465)
    server.ehlo()
    #server.starttls()
    server.ehlo()
    server.login('XXXXX@gmail.com','xxxxxxxxxxxxxx')

    subject = "The Book you want is below $20! Now is your chance to buy!"
    body = "Larry, This is the moment we have been waiting for. Now is your chance to pick up the book Becoming a Data Head. Don't mess it up! Link here: https://www.amazon.com/Becoming-Data-Head-Understand-Statistics-ebook/dp/B092TSVZJ8/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr="
   
    msg = f"Subject: {subject}\n\n{body}"
    
    server.sendmail(
        'XXXXX@gmail.com',
        msg
    )

# Runs check_price after a set time and inputs data into the CSV
while(True):
    check_price()
    time.sleep(86400)
```

## Disclaimer

This project is inspired by [Alex The Analyst](https://www.youtube.com/@AlexTheAnalyst), and is intended for educational purposes only. Web scraping can violate the terms of service of some websites. Always ensure you are in compliance with the legalities and ethical considerations of web scraping for your use case.
