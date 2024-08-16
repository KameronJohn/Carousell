""" 
 
This program is essentially a tool designed to automate the process of collecting (or "scraping") new items listed on a website called Carousell, 
which is an online marketplace. It's specifically focused on finding listings for items related to Post Malone, a popular music artist. 
The program opens a browser automatically, goes to Carousell, looks for new listings, and saves details like the time the item was posted, 
the price, the title, and the URL of the listing.

Here’s a simple breakdown of what it does:

    Opens Carousell's Website: The program uses a browser to automatically go to Carousell and search for new items related to Post Malone.

    Scrolls and Looks for Listings: It scrolls through the page, looks at the listed items, and gathers important information such as:
        When the item was listed (time)
        The price of the item
        The title of the listing (like "Selling Post Malone Poster")
        The web address (URL) where the listing can be found

    Avoids Repeated Listings: 
        The program checks if it has already saved a listing before. If it finds a new listing, it saves it.

    Updates Google Spreadsheet: 
        When new listings are found, the program adds the information to a Google Spreadsheet (like an online Excel file). It tracks when it last updated this information.

    Handles Errors and Refreshes: 
        If the program gets stuck (like if it can’t load the page properly), it will try to reload and start again, making sure it can continue scraping.

    Repeats Until All Listings Are Scraped: 
        The program keeps scrolling and scraping until it reaches the end of the listings or a set limit is reached.

Why is This Useful?

This program saves time by automatically tracking and collecting all new listings related to a specific keyword instead of having to do it manually. It ensures that nothing is missed and that the information is stored neatly in a spreadsheet for easy reference.

"""
import AUTOsubananaNEWNEWNEW
from undetected_chromedriver       import Chrome
from selenium.webdriver.common.by  import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support    import expected_conditions as EC
from selenium.common import exceptions
from selenium.webdriver.common.keys import Keys
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.action_chains import ActionChains
from webdriver_manager.chrome import ChromeDriverManager
from selenium import webdriver
import datetime
import os
import pandas as pd
import AUTOgoogleSpreadsheet
import re
from time import sleep
import keyboard
""" 
scraping for new listed items
"""
class carousell:
    def __init__(self) -> None:
        self.times_clicking_show_more_results = 15
        """  """
        self.current_location = os.path.abspath(os.path.dirname(__file__))
        self.current_location += '\\'
        self.outputPath = f"{self.current_location}excel"+"\\"
        self.ss = AUTOgoogleSpreadsheet.ss()
        self.ss.connect_google()
        self.spreadsheet_id = "18q--1CTwALJGStGXEwhs5lNvLJk4kBU7ATfVXUG4N1A"
        self.spreadsheet_number = "Sheet1"
        self.df = self.ss.read_sheet(self.spreadsheet_id, self.spreadsheet_number)
        # print(self.df.tail(5))
    def get_url(self):
        chrome_driver_path = self.current_location+"chromedriver.exe"
        options = Options()
        options.add_argument('--lang=en')
        self.driver = webdriver.Chrome(ChromeDriverManager().install())
        # self.driver = webdriver.Chrome()
        # self.driver = Chrome(executable_path=chrome_driver_path,options=options)
        self.url  =r"https://www.carousell.com.hk/search/post%20malone?addRecent=true&canChangeKeyword=true&includeSuggestions=true&searchId=SEpQO-&t-search_query_source=direct_search"
        self.driver.get(self.url)
    def testing(self):
        print(self.df["4"].values)
    def getDetails(self,refresh=False):
        """ scroll to the first element: start """
        # The WebElement you want to scroll to
        # main_box = '//*[@class="D_za M_tK"]'
        main_box = '//*[contains(@class, "D_xy M_sH")]'
        main_box = "//div[starts-with(@data-testid, 'listing-card-')]"
        columns=['time', 'price','title','url']

        import time
        start_time = time.time()
        while True:
            try:
                scrollTo = self.driver.find_element(By.XPATH, main_box)
                break
            except Exception as e:
                pass
            count += 1
            elapsed_time = time.time() - start_time
            if elapsed_time > 30:
                self.reset_page()
                print('OVER 30 SECONDS!!!!')
                start_time = time.time()
                # print("An error occurred:", e)
                # main_box = input("intput new main to continue...")

        js_code = "arguments[0].scrollIntoView();"
        # Execute the JS script
        self.driver.execute_script(js_code, scrollTo)
        """ scroll to the first element: end """
        output = dict()
        bottomCount = 0
        count = 0
        bottomed_reach = 0
        while True:
            # in to execute_script method
            js_code = "arguments[0].scrollIntoView();"

            # The WebElement you want to scroll to
            try:
                last_element
   
                scrollTo = self.driver.find_element(By.CSS_SELECTOR, f'[data-testid="{last_element}"]')
                # Execute the JS script
                self.driver.execute_script(js_code, scrollTo)
            except UnboundLocalError:
                last_element = 'abd'
            except NoSuchElementException:
                pass
                # Handle the exception (e.g. print an error message)
                # print("Element not found on the page")
                # return
            
            #the main element blocks
            elements = self.driver.find_elements(By.XPATH, main_box)
            # print(elements)
            elements = elements[-60:]
            for i,element in enumerate(elements):
                while True:
                    try:
                        lastestId = element.get_attribute("data-testid")
                        break
                    except:
                        pass
                if lastestId not in output:
                    textList = []
                    #time
                    try:
                        descendant_elements = element.find_element(By.XPATH, './/div/a/div[2]/div[1]/p')
                        time = descendant_elements.text
                    except:
                        time = 'unknown'
                    textList.append(time)

                    #price
                    descendant_elements = element.find_element(By.XPATH, './/div/a[2]/div[2]/p')
                    price = descendant_elements.text
                    textList.append(price)

                    #title
                    descendant_elements = element.find_element(By.XPATH, ".//div/a[2]/p[1]")
                    title = descendant_elements.text
                    number = 0
                    if '收' in title:
                        number = 1
                    elif '徵' in title:
                        number = 1
                    elif 'want' in title:
                        number = 1
                    elif '求' in title:
                        number = 1
                    textList.append(number)
                    textList.append(title)


                    #url
                    url = element.find_element(By.XPATH, './/div/a[2]').get_attribute("href")
                    textList.append(url)

                    output[lastestId] = textList
                else:
                    count +=1
                    # print('included: ',str(count))
            if lastestId == last_element: 
                bottomCount +=1
                if bottomCount ==2:
                    self.df = self.ss.read_sheet(self.spreadsheet_id, self.spreadsheet_number)
                    bottomed_reach +=1
                    bottomCount = 0
                    tmp_list = []
                    for lastestId,textList in output.items():
                        if textList[4] not in self.df["4"].values:
                            if textList[3] not in self.df["3"].values:
                                current_time = datetime.datetime.now()
                                formatted_time = current_time.strftime("%d/%m_%H:%M:%S:%f")[:-3]
                                textList.append(formatted_time)
                                textList.append("99999")
                                tmp_list.append(textList)
                                print('   -'*20)
                                print("new: ", f"{textList[0]}: {textList[3]}\n{textList[4]}")
                                print('   -'*20)
                    if len(tmp_list) != 0:
                        next_row_index = len(self.df)+1
                        self.ss.write_sheet(next_row_index,tmp_list)
                        subject = 'The spreadsheet was updated at ' + str(datetime.datetime.now())

                        tmp_list.clear()
                    # Export the DataFrame to an Excel file
                    # df.to_excel('first_trial.xlsx', index=False)
                    xpath_query = '//button[@type="button" and contains(text(), "Show more results")]'
                    try:
                        last_element = ""
                        element = self.driver.find_element(By.XPATH, xpath_query)
                        element.click()
                        sleep(2)
                        scrollTo = self.driver.find_element(By.CSS_SELECTOR, f'[data-testid="{last_element}"]')
                        # Execute the JS script
                        self.driver.execute_script(js_code, scrollTo)
                    except:
                        pass
                    """ times that clicking Show more results """
                    if bottomed_reach % self.times_clicking_show_more_results ==0:
                        if refresh is True:
                            self.reset_page()
                            print("refresh done")
                            now = datetime.datetime.now()
                            current_time = now.strftime("%d/%m/%Y %H:%M:%S")
                            self.ss.write_sheet(1,[current_time,''],worksheet_name="Sheet2")
                            print('.',end="")
                            output.clear()
                            sleep(10)
            last_element = lastestId
        sleep(99999)
    def reset_page(self):
        while True:
            try:
                self.driver.get(r"https://www.carousell.com.hk/")
                sleep(5)
                self.driver.get(self.url)
                break
            except:
                print('oh no')
                sleep(10)
    def list_url(self):
        """ selenium version has to be 4.2.0 """
        chrome_driver_path = self.current_location+"chromedriver.exe"
        options = Options()
        options.add_argument('--lang=en')
        self.driver = webdriver.Chrome(ChromeDriverManager().install())
        # self.driver = Chrome(executable_path=chrome_driver_path,options=options)
        # self.driver = webdriver.Chrome()
        url_list = []
        for i in url_list:
            self.driver.get(i)
            while True:
                if keyboard.is_pressed('F7'):
                    try:
                        query = '//*[text()="對話"]/..'
                        query = '//*[text()="Chat"]/..'
                        element = self.driver.find_elements(By.XPATH, query)
                        element[1].click()
                    except:
                        print('cant find')
                if keyboard.is_pressed('F8'):
                    break

def main():
    c = carousell()
    c.list_url()
    # c.get_url()
    # c.getDetails(True)
if __name__ == "__main__":
    # excel()
    main()
