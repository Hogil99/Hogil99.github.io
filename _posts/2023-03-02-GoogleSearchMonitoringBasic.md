날짜별로 나만의 관심주 기사 모으기 완성

뭐징

import requests
from bs4 import BeautifulSoup
from urllib.parse import parse_qs, urlparse

url = "https://www.google.com/search?q=에코프로비엠+after:2023-02-28+before:2023-03-2&tbm=nws"

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"
}

response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.text, "html.parser")

seen_links = set()
titles = soup.select(".nDgy9d")
links = []
for i, title in enumerate(titles):
    link = title.find_parent("a")["href"]
    if "https://www.google.com/url?q=" in link:
        parsed_link = parse_qs(urlparse(link).query)["url"][0]
        link = parsed_link
    if len(links) > 0 and links[-1] != link:
        print(links[-1])
        print()
    print(title.text)
    links.append(link)
print(links[-1])
