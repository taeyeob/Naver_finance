## 목차
1. 네이버 증권 모바일 사이트에서 ETF 섹터별 종가 불러오기


## 소스코드
1. 저번 macro data 폴더(크롬 드라이버가 있는 폴더)에 **naverfi.py** 파일을 생성합니다.
2. 다음 코드를 복사, 붙여넣기 합니다.
3. MORT의 경우 사이트 주소가 MORT.K 로 끝나기 때문에 cmd에 입력시 **mort.k** 로 입력하여야 합니다.
4. 비슷한 경우가 또 발생할 수 있기 때문에 잘 확인해야 합니다.
```Python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
import csv
import pandas as pd

print("Ticekrs: (단 MORT의 경우 MORT.K로 입력할 것")
tickers = input().split()

path = '/Users/TaeyeobKim/Desktop/EPIAdvisor/MacroDatas/chromedriver'
driver = webdriver.Chrome(path)

for i in range(len(tickers)):
    driver.implicitly_wait(3)
    driver.get("https://m.stock.naver.com/worldstock/index.html#/etf/" + tickers[i])
    
    table = driver.find_element_by_xpath('//*[@id="content"]/div[10]/div[2]')
    rows = table.find_elements_by_tag_name('tr')
    
    output_rows = []
    for row in rows:
        columns = row.find_elements_by_tag_name('td')
        output_row = []
        for column in columns:
            output_row.append(column.text)
        output_rows.append(output_row)
    
    if i == 0:
        with open('navfi.csv', 'w') as csvfile:
            writer = csv.writer(csvfile)
            writer.writerow(tickers[i])
            writer.writerows(output_rows)
    else:
        with open('navfi.csv', 'a') as csvfile:
            writer = csv.writer(csvfile)
            writer.writerow(tickers[i])
            writer.writerows(output_rows)
```
##### 해당 사용설명서대로 진행되지 않을 경우 연락 주시길 바랍니다.
##### 만일 **This version of ChromeDriver only supports Chrome version 83** 이라는 오류 문구가 명령 프롬프트 창에 보일 시 기존 Chrome driver를 삭제 후 재설치하여 같은 폴더에 넣으시면 됩니다(Macro Datas 매뉴얼 참고).