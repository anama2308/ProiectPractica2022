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
