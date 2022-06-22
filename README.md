# ProiectPractica2022


# Day1 - permisie

# Day2 - Mi am ales tema si am inceput sa ma documentez despre ea de pe site-urile : https://www.analyticsvidhya.com/blog/2020/10/web-scraping-selenium-in-python/, https://scrapfly.io/blog/web-scraping-with-selenium-and-python/, https://www.webscrapingapi.com/python-selenium-web-scraper/. Mi-am instalat python pe masina viruala, am instalat selenium si panas ruland succesiv comenzile:
# sudo apt install python
# pip3 install selenium
# pip3 install pandas
# Mi-am descarcat google chrome si driver-ul pentru el.

# Day3 - Am vizionat tutorialele: https://www.youtube.com/watch?v=b5jt2bhSeXs si am executat codurile pentru a intelege cum functioneaza web scrapingul

from lib2to3.pgen2 import driver
import click
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


#locatia driver ului chrome
path = "/usr/local/bin/chromedriver" 

driver = webdriver.Chrome(path) 

#se acceseaza site ul

driver.get("https://techwithtim.net") 

#print(driver.title) # afiseaza titlul website ul accesat

#search = driver.find_element(By.NAME,"s")
#search.send_keys("test")
#search.send_keys(Keys.RETURN)

#print(driver.page_source)

link = driver.find_element(By.LINK_TEXT, "Python Programming")
link.click()

#prin try..except/ try.. finally navigam in interoriul site ului web 


try:
    element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.LINK_TEXT, "Beginner Python Tutorials"))
    )
    #element.clear() #inainte de a trimite key verifica daca nu exista alta, iar daca exista o sterge, altfel ar fi facu append
    element.click()

    element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "sow-button-19310003"))
    )
    element.click() #accesez un anumit link(de preferat)

    driver.back()  #revin la pagina anterioara

    #driver.forward()

#    main = WebDriverWait(driver, 10).until(
#        EC.presence_of_element_located((By.ID, "main"))
#    
#    # print(main.text)
#  
#    )
#     
#   articles = main.find_elements(By.TAG_NAME, "article")
#    for article in articles:
#        header = article.find_element(By.CLASS_NAME, "entry-summary")
#        print(header.text)
#
except:
    driver.quit()



#time.sleep(5) #mentine deschisa pagina cat timp dorim noi

#driver.close() # inchide fereastra curenta
#driver.quit() # opreste browser ul complet 

from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.keys import Keys
import time

from selenium.webdriver.common.action_chains import ActionChains

path = "/usr/local/bin/chromedriver" 

driver = webdriver.Chrome(path) 
driver.get("https://orteil.dashnet.org/cookieclicker/")

driver.implicitly_wait(5) # asteapta cat are scris in paranteaza pana sa continue actiunile

cookie = driver.find_element_by_id("bigCookie")
cookie_count = driver.find_element_by_id("cookies")

# se creaza o lista care sa aibe anumite atribute
items = [driver.find_element_by_id("productPrice" + str(i)) for i in range(1,-1,-1)]

# coada care retine actiunile pe care vrem sa le facem  
actions = ActionChains(driver)

# pentru a folosi comanfa anterioara trebuie asa apelam fct perform pt a avea efect asupra programului

for i in range(50): # executa instructiunea de 5000 ori
    actions.click(cookie) 
    actions.perform()
    count = int(cookie_count.text.split(" ")[0])
    for item in items:
        value=int(item.text)
        if value <= count :
            upgrade_action = ActionChains(driver)
            upgrade_action.move_to_element(item)
            upgrade_action.click()
            upgrade_action.perform()


#time.sleep(10)

# Day4 - Am continuat de vizionat si testat exemplele din tutoriale

# main.py

import unittest
from selenium import webdriver
import page
from selenium.webdriver.chrome.service import Service


from lib2to3.pgen2 import driver
import click

from selenium.webdriver.common.keys import Keys
import time
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

path = Service('/usr/local/bin/chromedriver')

class PythonOrgSearch(unittest.TestCase):
    
    def setUp(self):
        self.driver = webdriver.Chrome(service=path)
        self.driver.get("http://www.python.org")
       

    def test_search_python(self):
        mainPage = page.mainPage(self.driver)
        assert mainPage.is_title_matches()
        mainPage.search_text_element = "pycon"
        mainPage.click_go_button()
        search_result_page  = page.searchResultPage(self.driver)
        assert search_result_page.is_result_found()

    def tearDown(self):
        self.driver.close()    


if __name__ == "__main__":
    unittest.main()
    
 # page.py
 
 from lib2to3.pgen2 import driver
from locator import *
from elements import basePageElement


class searchTextElement(basePageElement):
    locator = "q"


class basePage(object):
    def __init__(self):
        self.driver = driver

class mainPage(basePage): 

    search_text_element = searchTextElement()

    def is_title_matches(self):
        return "Python" in self.driver.title

    def click_go_button(self):
        element = self.driver.find_element(*mainPageLoators.GO_BUTTOM)
        element.click()

class searchResultPage(basePage):

    def is_result_found(self):
        return "No results found" not in self.driver.page_source

# elements.py

from lib2to3.pgen2 import driver
from selenium.webdriver.support.ui import WebDriverWait


class basePageElement(object):

    def __set__(self, obj, value):
        driver = obj.driver
        WebDriverWait(driver,100).until( 
            lambda driver: driver.find_element_by_name(self.locator)
        )
        driver.find_element_by_name(self.locator).clear()
        driver.find_element_by_name(self.locator).send_keys(value)

    def __get__(self, obj, owner):
        driver = obj.driver
        WebDriverWait(driver,100).until( 
            lambda driver: driver.find_element_by_name(self.locator)
        )
        element = driver.find_element_by_name(self.locator)
        return element.get_attribute("value")

# locator.py

from selenium.webdriver.common.by import By


class mainPageLoators(object):
    GO_BUTTOM = (By.ID, "submit")


class searchResultsPageLocators(object):
    pass 
# Day5 - M-am hotarat sa fac web scraping pentru preturile zborurilor. M-am documentant si am gasit acest cod pentru a ma ajuta :
from time import sleep, strftime
from random import randint
import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import smtplib
from email.mime.multipart import MIMEMultipart



class Flight_Bot:

    
    def __init__(self):
        print('initiating the bot...')
        
        self.YOUR_HOTMAIL_EMAIL = # string
        self.YOURPASSWORD = # string
        self.YOUR_OTHER_EMAIL = # string
        YOUR_CHROME_DRIVER_PATH = # string
        chromedriver_path = YOUR_CHROME_DRIVER_PATH + '/chromedriver_win32/chromedriver.exe'
        global driver
        driver = webdriver.Chrome(executable_path=chromedriver_path) # This will open the Chrome window
        sleep(2)
        
    
    # Load more results to maximize the scraping
    def load_more(self):
        try:
            more_results = '//a[@class = "moreButton"]'
            driver.find_element_by_xpath(more_results).click()
            # Printing these notes during the program helps me quickly check what it is doing
            print('sleeping.....')
            sleep(randint(45,60))
        except:
            pass
    
    def page_scrape(self):
        """This function takes care of the scraping part"""
        sleep(5)
        xp_sections = '//*[@class="section duration"]'
        sections = driver.find_elements_by_xpath(xp_sections)
        sections_list = [value.text for value in sections]
        section_a_list = sections_list[::2] # This is to separate the two flights
        section_b_list = sections_list[1::2] # This is to separate the two flights
        
        # if you run into a reCaptcha, you might want to do something about it
        # you will know there's a problem if the lists above are empty
        # this if statement lets you exit the bot or do something else
        # you can add a sleep here, to let you solve the captcha and continue scraping
        # i'm using a SystemExit because i want to test everything from the start
        if section_a_list == []:
            raise SystemExit
        
        # I'll use the letter A for the outbound flight and B for the inbound
        a_duration = []
        a_section_names = []
        for n in section_a_list:
            # Separate the time from the cities
            a_section_names.append(''.join(n.split()[2:5]))
            a_duration.append(''.join(n.split()[0:2]))
        b_duration = []
        b_section_names = []
        for n in section_b_list:
            # Separate the time from the cities
            b_section_names.append(''.join(n.split()[2:5]))
            b_duration.append(''.join(n.split()[0:2]))
    
        xp_dates = '//div[@class="section date"]'
        dates = driver.find_elements_by_xpath(xp_dates)
        dates_list = [value.text for value in dates]
        a_date_list = dates_list[::2]
        b_date_list = dates_list[1::2]
        # Separating the weekday from the day
        a_day = [value.split()[0] for value in a_date_list]
        a_weekday = [value.split()[1] for value in a_date_list]
        b_day = [value.split()[0] for value in b_date_list]
        b_weekday = [value.split()[1] for value in b_date_list]
        print(b_weekday)
        
        # getting the prices
        xp_prices = '//a[@class="booking-link"]/span[@class="price option-text"]'
        prices = driver.find_elements_by_xpath(xp_prices)
        prices_list = [price.text.replace('$','') for price in prices if price.text != '']
        prices_list = list(map(int, prices_list))
        print(prices_list)
    
        # the stops are a big list with one leg on the even index and second leg on odd index
        xp_stops = '//div[@class="section stops"]/div[1]'
        stops = driver.find_elements_by_xpath(xp_stops)
        stops_list = [stop.text[0].replace('n','0') for stop in stops]
        a_stop_list = stops_list[::2]
        b_stop_list = stops_list[1::2]
    
        xp_stops_cities = '//div[@class="section stops"]/div[2]'
        stops_cities = driver.find_elements_by_xpath(xp_stops_cities)
        stops_cities_list = [stop.text for stop in stops_cities]
        a_stop_name_list = stops_cities_list[::2]
        b_stop_name_list = stops_cities_list[1::2]
        
        # this part gets me the airline company and the departure and arrival times, for both legs
        xp_schedule = '//div[@class="section times"]'
        schedules = driver.find_elements_by_xpath(xp_schedule)
        hours_list = []
        carrier_list = []
        for schedule in schedules:
            hours_list.append(schedule.text.split('\n')[0])
            carrier_list.append(schedule.text.split('\n')[1])
        # split the hours and carriers, between a and b legs
        a_hours = hours_list[::2]
        a_carrier = carrier_list[::2]
        b_hours = hours_list[1::2]
        b_carrier = carrier_list[1::2]
    
        
        cols = (['Out Day', 'Out Time', 'Out Weekday', 'Out Airline', 'Out Cities', 'Out Duration', 'Out Stops', 'Out Stop Cities',
                'Return Day', 'Return Time', 'Return Weekday', 'Return Airline', 'Return Cities', 'Return Duration', 'Return Stops', 'Return Stop Cities',
                'Price'])
    
        flights_df = pd.DataFrame({'Out Day': a_day,
                                   'Out Weekday': a_weekday,
                                   'Out Duration': a_duration,
                                   'Out Cities': a_section_names,
                                   'Return Day': b_day,
                                   'Return Weekday': b_weekday,
                                   'Return Duration': b_duration,
                                   'Return Cities': b_section_names,
                                   'Out Stops': a_stop_list,
                                   'Out Stop Cities': a_stop_name_list,
                                   'Return Stops': b_stop_list,
                                   'Return Stop Cities': b_stop_name_list,
                                   'Out Time': a_hours,
                                   'Out Airline': a_carrier,
                                   'Return Time': b_hours,
                                   'Return Airline': b_carrier,                           
                                   'Price': prices_list})[cols]
        
        flights_df['timestamp'] = strftime("%Y%m%d-%H%M") # so we can know when it was scraped
        return flights_df
    
    
    def start_kayak(self, city_from, city_to, date_start, date_end):
        """City codes - it's the IATA codes!
        Date format -  YYYY-MM-DD"""
    
        kayak = ('https://www.kayak.com/flights/' + city_from + '-' + city_to +
                 '/' + date_start + '-flexible/' + date_end + '-flexible?sort=bestflight_a')
        driver.get(kayak)
        sleep(randint(15,20))
        
        # sometimes a popup shows up, so we can use a try statement to check it and close
        try:
            xp_popup_close = '//button[contains(@id,"dialog-close") and contains(@class,"Button-No-Standard-Style close ")]'
            driver.find_elements_by_xpath(xp_popup_close)[5].click()
        except Exception as e:
            pass
        sleep(randint(15,25))
        print('loading more.....')
        
    #     Flight_Bot.load_more(self)
        
        print('starting first scrape.....')
        df_flights_best = Flight_Bot.page_scrape(self)
        df_flights_best['sort'] = 'best'
        sleep(randint(18,25))
        df_flights_best.to_csv('testing_best.csv')
        
        try:
            # Let's also get the lowest prices from the matrix on top
            matrix = driver.find_elements_by_xpath('//*[contains(@id,"FlexMatrixCell")]')
            matrix_prices = [price.text.replace('$','') for price in matrix]
            matrix_prices = list(map(int, matrix_prices))
            matrix_min = min(matrix_prices)
            matrix_avg = sum(matrix_prices)/len(matrix_prices)
        except Exception as e:
            driver.find_elements_by_xpath('//button[contains(@class,"heading withBorder Button-No-Standard-Style")]')[1].click()
            # Let's also get the lowest prices from the matrix on top
            matrix = driver.find_elements_by_xpath('//*[contains(@id,"FlexMatrixCell")]')
            matrix_prices = [price.text.replace('$','') for price in matrix]
            matrix_prices = list(map(int, matrix_prices))
            matrix_min = min(matrix_prices)
            matrix_avg = sum(matrix_prices)/len(matrix_prices)
            pass            
            
        
        print('switching to cheapest results.....')
        cheap_results = '//a[@data-code = "price"]'
        driver.find_element_by_xpath(cheap_results).click()
        sleep(randint(60,90))
        print('loading more.....')
        
    #     Flight_Bot.load_more(self)
        
        print('starting second scrape.....')
        df_flights_cheap = Flight_Bot.page_scrape(self)
        df_flights_cheap['sort'] = 'cheap'
        sleep(randint(60,80))
        
        print('switching to quickest results.....')
        quick_results = '//a[@data-code = "duration"]'
        driver.find_element_by_xpath(quick_results).click()  
        sleep(randint(60,90))
        print('loading more.....')
        
    #     Flight_Bot.load_more(self)
        
        print('starting third scrape.....')
        df_flights_fast = Flight_Bot.page_scrape(self)
        df_flights_fast['sort'] = 'fast'
        sleep(randint(60,80))
        
        # saving a new dataframe as an excel file. the name is custom made to your cities and dates
        final_df = df_flights_cheap.append(df_flights_best).append(df_flights_fast)
        final_df.to_excel('search_backups//{}_flights_{}-{}_from_{}_to_{}.xlsx'.format(strftime("%Y%m%d-%H%M"),
                                                                                       city_from, city_to, 
                                                                                       date_start, date_end), index=False)
        print('saved df.....')
        
        # We can keep track of what they predict and how it actually turns out!
        xp_loading = '//div[contains(@id,"advice")]'
        loading = driver.find_element_by_xpath(xp_loading).text
        xp_prediction = '//span[@class="info-text"]'
        prediction = driver.find_element_by_xpath(xp_prediction).text
        print(loading+'\n'+prediction)
        
        # sometimes we get this string in the loading variable, which will conflict with the email we send later
        # just change it to "Not Sure" if it happens
        weird = '¯\\_(ツ)_/¯'
        if loading == weird:
            loading = 'Not sure'
        
        username = self.YOUR_HOTMAIL_EMAIL
        password = self.YOURPASSWORD
    
        server = smtplib.SMTP('smtp.outlook.com', 587)
        server.ehlo()
        server.starttls()
        server.login(username, password)
        msg = ('Subject: Flight Scraper\n\n\
    Cheapest Flight: {}\nAverage Price: {}\n\nRecommendation: {}\n\nEnd of message'.format(matrix_min, matrix_avg, (loading+'\n'+prediction)))
        message = MIMEMultipart()
        message['From'] = self.YOUR_HOTMAIL_EMAIL
        message['to'] = self.YOUR_OTHER_EMAIL
        server.sendmail(self.YOUR_HOTMAIL_EMAIL, self.YOUR_OTHER_EMAIL, msg)
        print('sent email.....')
        
  # Insa in timp ce rulam imi apareau erori legate de biblioteci, le am instalat, dezinstalat si reinstalat, iar erorile continuau sa apara. Am cautat solutii pe net, am incercat mai multe variante, dar printre toate comenzile date, am reusit sa imi stric masina virtuala. Am fost nevoita sa o dezinstalez, sa o reinstales si sa reiau procesul de instalare al aplicatiilor necesare pentru proiectul meu 



# Day6 - Discutand cu o colega, am aflat ca avem aceeasi tema, fapt ce a determinat sa ma razgandesc. Am decis sa fac web scraping pe un dictionar online. Am inceput prin a face interfata. 
# Pentru a realiza interfata ma folosesc de biblioteca tkinter, iar pentru asta am folosit urmatoarea documentatie: https://youtu.be/z1nN5pvhdA8, https://docs.python.org/3/library/tk.html, https://stackoverflow.com/questions/3352918/how-to-center-a-window-on-the-screen-in-tkinter, https://www.pythontutorial.net/tkinter/tkinter-stringvar/, https://www.geeksforgeeks.org/how-to-use-images-as-backgrounds-in-tkinter/


# Codul realizat de mine:

from email import header
from struct import pack
import tkinter as tk
from PIL import Image, ImageTk

def closeApp():
    window.destroy

window = tk.Tk()

window.title("Dictionary")
window.geometry("1000x600")

bg = ImageTk.PhotoImage(file='book.png')

label1 = tk.Label(window, image = bg)
label1.place(x = 0, y = 0)
  
label2 = tk.Label(window, text = "Explore Definition & Meaning", height='3', width='40', font=("Calibri 16 bold italic"))
label2.pack(pady = 50)


frame1 =  tk.Frame(window)
frame1.pack(pady=20)

introduceWord = tk.Label(frame1, text='Word: ')
introduceWord.grid(column=0, row=0, padx=5, pady=5)

word = tk.StringVar()
wordEntry = tk.Entry(frame1, textvariable=word, width=10)
wordEntry.focus()
wordEntry.grid(column=1, row=0, padx=5, pady=5)

frame2 = tk.Frame(window)
frame2.pack(pady = 40)

button1 = tk.Button(frame2,text="Search")
button1.pack(side='bottom')
  

window.mainloop()
