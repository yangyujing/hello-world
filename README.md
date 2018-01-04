import requests
from bs4 import BeautifulSoup
import time

headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36'}

def get_info(url):
    wb_data=requests.get(url,headers=headers)
    soup=BeautifulSoup(wb_data.text,'lxml')
    ranks=soup.select( 'span.pc_temp_num ')
    tittles=soup.select('div.pc_temp_songlist > ul > li > a')
    times=soup.select(' span.pc_temp_tips_r > span')
    for rank,tittle,time in zip(ranks,tittles,times):
        data = {
        'rank':rank.get_text().strip(),
        'singer':tittle.get_text().split('-')[0],
        'song':tittle.get_text().split('-')[0],
        'time':time.get_text().strip()
    }
        print(data)
if __name__ == '__main__':
    urls = ['http://www.kugou.com/yy/rank/home/{}-8888.html'.format(str(i)) for i in range(1,25)]
    for url in urls:
        get_info(url)
    time.sleep(1)
