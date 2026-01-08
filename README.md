# Amazon Scraper

[![Promo](https://github.com/bright-kr/Amazon-scraper/blob/main/images/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.co.kr/products/web-scraper/amazon?promo=github15) 

## Table of Contents

- [무료 Amazon Scraper](#free-amazon-scraper)
   - [사전 요구 사항](#prerequisites)
   - [빠른 설정](#quick-setup)
   - [Amazon 데이터를 スクレイピング하는 방법](#how-to-scrape-amazon-data)
   - [출력](#output)
- [Amazon 데이터를 スクレイピング할 때의 과제](#challenges-when-scraping-amazon-data)
- [솔루션: Bright Data Amazon Scraper API](#solution-bright-data-amazon-scraper-api)
- [Amazon Scraper API 실제 사용 예](#amazon-scraper-api-in-action)
   - [API パラメータ로 데이터 수집 사용자 지정](#customize-data-collection-with-api-parameters)
   - [Amazon 제품 데이터](#amazon-product-data)
   - [Amazon 리뷰 데이터](#amazon-reviews-data)
   - [Amazon 제품 검색](#amazon-products-search)
   - [Amazon 판매자 정보](#amazon-sellers-info)
   - [베스트셀러 기준 Amazon 제품](#amazon-products-by-best-sellers)
   - [카테고리 URL 기준 Amazon 제품](#amazon-products-by-category-url)
   - [키워드 기준 Amazon 제품](#amazon-products-by-keyword)
   - [Amazon 제품 글로벌 データセット](#amazon-products-global-dataset)
   - [Amazon 제품 글로벌 データセット - 카테고리 URL로 발견](#amazon-products-global-dataset---discover-by-category-url)
   - [Amazon 제품 글로벌 データセット - 키워드로 발견](#amazon-products-global-dataset---discover-by-keywords)


## Free Amazon Scraper
이 무료 도구를 사용하여 검색 결과 페이지에서 Amazon 제품 데이터를 직접 추출할 수 있습니다. 몇 가지 간단한 단계만으로 제품 제목, 가격, 평점, 리뷰 등 다양한 정보를 손쉽게 가져올 수 있습니다.

### Prerequisites
- Python 3.11 이상.
- 필요한 의존성을 설치합니다(아래 단계 참조).

### Quick Setup
1. 터미널을 열고 이 프로젝트 디렉터리로 이동합니다.
2. 다음 명령을 실행하여 의존성을 설치합니다:
   
    ```bash
    pip install -r requirements.txt
    ```

### How to Scrape Amazon Data
Amazon 데이터를 スクレイピング하려면 검색 쿼리만 제공하면 됩니다. 또한 Amazon 도메인과 スクレイピング할 페이지 수를 지정할 수도 있습니다.

#### Command:
```bash
python main.py "<your_search_query>" --domain="<amazon_domain>" --pages=<number_of_pages>
```
- `<your_search_query>`: 검색 키워드(예: "coffee maker").
- `<amazon_domain>`: スクレイピング할 Amazon 도메인(기본값: Amazon US의 `com`).
- `<number_of_pages>`: スクレイピング할 페이지 수(선택 사항이며, 기본값은 사용 가능한 모든 페이지를 スクレイピング합니다).

#### Example:
Amazon US 도메인에서 "coffee maker" 데이터를 スクレイピング하고 결과의 처음 3페이지를 スクレイ핑하려면 다음 명령을 사용합니다.
명령은 다음과 같습니다:
```bash
python main.py "coffee maker" --domain="com" --pages=3
```
### Output
スクレイピング 후 추출된 데이터는 프로젝트 디렉터리에 `amazon_data.csv`로 저장됩니다. CSV 파일에는 다음 세부 정보가 포함됩니다:
- **Name:** 제품 제목.
- **Current Price:** 제품 가격(품절인 경우 비어 있음).
- **Rating:** 평균 고객 평점.
- **Reviews:** 고객 리뷰 총 개수.
- **ASIN:** Amazon Standard Identification Number.
- **Link:** Amazon의 제품 페이지로 바로 가는 URL.

데이터는 다음과 같이 표시됩니다:

<img width="700" alt="bright-data-amazon_csv_data" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-amazon_csv_data.png">

## Challenges When Scraping Amazon Data
Amazon 데이터를 スクレイピング하는 일은 항상 간단하지는 않습니다. 다음은 마주칠 수 있는 몇 가지 과제입니다:
1. **고도화된 アンチボット 조치:** Amazon은 CAPTCHA, 보이지 않는 봇 탐지 기법, 행동 분석(예: 마우스 움직임 추적 등)을 사용하여 봇을 차단합니다.
2. **빈번한 페이지 구조 업데이트:** Amazon은 HTML 구조, ID, class 이름을 자주 변경하므로 새로운 페이지 레이아웃에 맞추기 위해 スクレイ퍼를 정기적으로 업데이트해야 합니다.
3. **높은 리소스 사용량:** Playwright 또는 Selenium 같은 도구로 JavaScript 비중이 큰 페이지를 スクレイピング하면 상당한 시스템 리소스를 소모할 수 있습니다. 동적 콘텐츠를 처리하고 여러 브라우저 인스턴스를 실행하면 특히 대량의 데이터를 スクレイピング할 때 성능이 저하될 수 있습니다.

아래는 Amazon이 자동화된 スクレイピング 시도를 감지했을 때 발생하는 예시입니다:

<img src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/Amazon%20Blocked.png" alt="Amazon Blocked" width="700"/>

위에서 보듯이 Amazon은 추가 데이터 スクレイピング을 방지하기 위해 리クレイスト를 차단했습니다. 이는 많은 スクレイ퍼가 겪는 일반적인 문제입니다.

## Solution: Bright Data Amazon Scraper API
[Bright Data Amazon Scraper API](https://brightdata.co.kr/products/web-scraper/amazon)는 대규모로 Amazon 제품 데이터를 スクレイピング하기 위한 궁극적인 솔루션입니다. 그 이유는 다음과 같습니다:

- **インフラ 관리 불필요**: プロキシ 또는 언블로킹 시스템을 처리할 필요가 없습니다.
- **ジオロケーション スクレイピング**: 어떤 지리적 지역에서도 スクレイピング할 수 있습니다.
- **글로벌 IP 커버리지**: 99.99% 업타임으로 [195개 국가](https://brightdata.co.kr/locations)의 [7,200만 개 이상의 실제 사용자 IP](https://brightdata.co.kr/proxy-types/residential-proxies)에 액세스할 수 있습니다.
- **유연한 데이터 전달**: Amazon S3, Google Cloud, Azure, Snowflake 또는 SFTP를 통해 JSON, NDJSON, CSV, `.gz` 등의 형식으로 데이터를 받을 수 있습니다.
- **프라이버시 준수**: GDPR, CCPA 및 기타 데이터 보호 법률을 완전히 준수합니다.
- **24/7 지원**: 전담 지원 팀이 24시간 연중무휴로 API 관련 질문 또는 문제를 지원합니다.

또한 제품을 테스트하고 요구 사항에 얼마나 적합한지 확인할 수 있도록 **무료 API 호출 20회**를 제공합니다.

## Amazon Scraper API in Action

> Amazon Scraper API 설정에 대한 자세한 가이드는 [Step-by-Step Setup Guide](https://github.com/bright-kr/Amazon-scraper/blob/main/scraper_api_setup.md#amazon-reviews)를 확인하십시오.

### Customize Data Collection with API Parameters

다음 API パラメータ를 사용하여 데이터 수집을 사용자 지정할 수 있습니다:

| **Parameter**       | **Type**   | **Description**                                                                                   | **Example**                                           |
|---------------------|------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| `limit`             | `integer`  | 각 입력에 대해 반환되는 결과 수를 제한합니다.                                            | `limit=10`                                           |
| `include_errors`    | `boolean`   | 문제 해결을 위해 출력에 오류 보고서를 포함합니다.                                      | `include_errors=true`                                |
| `notify`            | `url`      | 수집이 완료되면 알림이 전송되는 URL입니다.                                  | `notify=https://notify-me.com/`                      |
| `format`            | `enum`     | 데이터 전달 형식입니다. 지원 형식: JSON, NDJSON, JSONL, CSV.                          | `format=json`                                        |

💡추가 전달 방법: [webhook](https://docs.brightdata.com/scraping-automation/web-data-apis/web-scraper-api/overview#via-webhook)을 통해 또는 [API](https://docs.brightdata.com/scraping-automation/web-data-apis/web-scraper-api/overview#via-api)를 통해 데이터를 전달하도록 선택할 수 있습니다.

### Amazon Product Data
제품 URL을 제공하여 Amazon의 상세 제품 데이터를 수집합니다.

<img width="700" alt="bright-data-web-scraper-api-amazon-product-data" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-amazon-product-data.png">

#### Key Input Parameters:
| Parameter | Type   | Description                    | Required |
|-----------|--------|--------------------------------|----------|
| `url`       | `string` | 데이터를 スクレイピング할 Amazon 제품 URL | Yes      |

#### Performance:
- 입력당 평균 응답 시간: 13초

#### Sample Output Data:
Amazon 제품 데이터를 スクレイピング한 후 받게 되는 출력의 예시는 다음과 같습니다:
```json
{
    "url": "https://www.amazon.com/KitchenAid-Protective-Dishwasher-Stainless-8-72-Inch/dp/B07PZF3QS3",
    "title": "KitchenAid All Purpose Kitchen Shears with Protective Sheath...",
    "seller_name": "Amazon.com",
    "brand": "KitchenAid",
    "description": "These all-purpose shears from KitchenAid are a valuable addition...",
    "initial_price": 11.99,
    "final_price": 8.99,
    "currency": "USD",
    "availability": "In Stock",
    "reviews_count": 77557,
    "rating": 4.8,
    "categories": [
        "Home & Kitchen",
        "Kitchen & Dining",
        "Kitchen Utensils & Gadgets",
        "Shears"
    ],
    "asin": "B07PZF3QS3",
    "images": [
        "https://m.media-amazon.com/images/I/41E7ALk+uXL._AC_SL1200_.jpg",
        "https://m.media-amazon.com/images/I/710B9HpzMPL._AC_SL1500_.jpg"
    ],
    "delivery": [
        "FREE delivery Friday, October 25 on orders shipped by Amazon over $35",
        "Or fastest Same-Day delivery Today 10 AM - 3 PM. Order within 4 hrs 46 mins"
    ]
}
```
#### Code Example:
아래는 Amazon 제품 데이터 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = (
        f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}"
    )

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        time.sleep(10)
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None


def store_data(data, filename="amazon_products_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "YOUR_API_TOKEN"
    DATASET_ID = "gd_l7q7dkf244hwjntr0"

    datasets = [
        {
            "url": "https://www.amazon.com/Quencher-FlowState-Stainless-Insulated-Smoothie/dp/B0CRMZHDG8"
        },
        {
            "url": "https://www.amazon.com/KitchenAid-Protective-Dishwasher-Stainless-8-72-Inch/dp/B07PZF3QS3"
        },
        {
            "url": "https://www.amazon.com/TruSkin-Naturals-Vitamin-Topical-Hyaluronic/dp/B01M4MCUAF"
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```
전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_products_data.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Reviews Data
제품 URL과 함께 기간, 키워드, スクレイピング할 리뷰 수 같은 특정 パラメータ를 제공하여 Amazon 리뷰를 수집합니다.

<img width="700" alt="bright-data-web-scraper-api-amazon-product-reviews" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-amazon-product-reviews.png">


#### Key Input Parameters:
| **Parameter**       | **Type**  | **Description**                                                                 | **Required** |
|---------------------|-----------|---------------------------------------------------------------------------------|--------------|
| `url`               | `string`  | 리뷰를 スクレイピング할 Amazon 제품 URL입니다.                             | Yes          |
| `days_range`        | `number`  | 리뷰 수집 시 고려할 과거 일수(제한이 없으면 비워 둡니다). | No           |
| `keyword`           | `string`  | 특정 키워드로 리뷰를 필터링합니다.                            | No           |
| `num_of_reviews`    | `number`  | スクレイピング할 리뷰 수(제공하지 않으면 사용 가능한 모든 리뷰를 スクレイピング합니다). | No           |

#### Performance:
- 입력당 평균 응답 시간: 1분 1초

#### Sample Output Data:
Amazon 리뷰를 スクレイピング할 때 받게 되는 출력의 예시는 다음과 같습니다:
```json
{
    "url": "https://www.amazon.com/RORSOU-R10-Headphones-Microphone-Lightweight/dp/B094NC89P9/",
    "product_name": "RORSOU R10 On-Ear Headphones with Microphone...",
    "product_rating": 4.5,
    "product_rating_object": {
        "one_star": 386,
        "two_star": 237,
        "three_star": 584,
        "four_star": 1493,
        "five_star": 7630
    },
    "rating": 5,
    "author_name": "Amazon Customer",
    "review_header": "Great Sound For the Price!",
    "review_text": "I bought these headphones twice...",
    "badge": "Verified Purchase",
    "review_posted_date": "September 7, 2024",
    "helpful_count": 3
}
```
#### Code Example:
아래는 Amazon 리뷰 데이터 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = (
        f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}"
    )

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        time.sleep(10)
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None


def store_data(data, filename="amazon_reviews_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "YOUR_API_TOKEN"
    DATASET_ID = "gd_le8e811kzy4ggddlq"

    datasets = [
        {
            "url": "https://www.amazon.com/RORSOU-R10-Headphones-Microphone-Lightweight/dp/B094NC89P9/",
            "days_range": 0,
            "num_of_reviews": 4,
            "keyword": "great",
        },
        {
            "url": "https://www.amazon.com/Solar-Eclipse-Glasses-Certified-Viewing/dp/B08GB3QC1H",
            "days_range": 0,
            "num_of_reviews": 4,
            "keyword": "",
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```
전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_reviews_data.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products Search
검색을 위한 키워드를 제공하여 Amazon 제품을 찾습니다.

<img width="700" alt="bright-data-web-scraper-api-keyword-search" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-keyword-search.png">

#### Key Input Parameters:
| Parameter         | Type    | Description                                 | Required |
|-------------------|---------|---------------------------------------------|----------|
| `keyword`         | string  | 제품을 검색하는 데 사용하는 키워드      | Yes      |
| `url`             | string  | 검색할 도메인 URL              | Yes      |
| `pages_to_search` | number  | 검색할 페이지 수        | No       |

#### Performance:
- 입력당 평균 응답 시간: 1초

#### Sample Output Data:
Amazon에서 제품 키워드 검색을 수행한 뒤 받게 되는 출력의 예시는 다음과 같습니다:
```json
{
    "asin": "B08H75RTZ8",
    "url": "https://www.amazon.com/Microsoft-Xbox-Gaming-Console-video-game/dp/B08H75RTZ8/ref=sr_1_1",
    "name": "Xbox Series X 1TB SSD Console - Includes Xbox Wireless Controller...",
    "sponsored": "false",
    "initial_price": 479,
    "final_price": 479,
    "currency": "USD",
    "sold": 2000,
    "rating": 4.8,
    "num_ratings": 28675,
    "variations": null,
    "badge": null,
    "brand": null,
    "delivery": ["FREE delivery"],
    "keyword": "X-box",
    "image": "https://m.media-amazon.com/images/I/616klipzdtL._AC_UY218_.jpg",
    "domain": "https://www.amazon.com/",
    "bought_past_month": 2000,
    "page_number": 1,
    "rank_on_page": 1,
    "timestamp": "2024-10-20T10:39:37.679Z",
    "input": {
        "keyword": "X-box",
        "url": "https://www.amazon.com",
        "pages_to_search": 1
    }
}
```
#### Code Example:
아래는 키워드를 기반으로 Amazon 제품 검색을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = (
        f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}"
    )

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_keywords_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "YOUR_API_TOKEN"
    DATASET_ID = "gd_lwdb4vjm1ehb499uxs"

    datasets = [
        {"keyword": "X-box", "url": "https://www.amazon.com", "pages_to_search": 1},
        {"keyword": "PS5", "url": "https://www.amazon.de"},
        {
            "keyword": "car cleaning kit",
            "url": "https://www.amazon.es",
            "pages_to_search": 4,
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```
전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_keywords_data.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Sellers Info
특정 판매자 URL을 제공하여 Amazon 판매자에 대한 상세 정보를 확인합니다.

<img width="700" alt="bright-data-web-scraper-api-seller-info" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-seller-info.png">


#### Key Input Parameters:
| **Parameter** | **Type**  | **Description**                    | **Required** |
|---------------|-----------|------------------------------------|--------------|
| `url`         | `string`  | Amazon 판매자 URL              | Yes          |

#### Performance:
- 입력당 평균 응답 시간: 1초

#### Sample Output Data:
판매자 정보를 スクレイピング한 후 받게 되는 출력의 예시는 다음과 같습니다:
```json
{
    "input": {
        "url": "https://www.amazon.com/sp?seller=A33W53J5GVPZ8K"
    },
    "seller_id": "A33W53J5GVPZ8K",
    "seller_name": "Peckomatic",
    "description": "Peckomatic is committed to providing each customer with the highest standard of customer service.",
    "detailed_info": [
        {"title": "Business Name"},
        {"title": "Business Address"}
    ],
    "stars": "4.5 out of 5 stars",
    "feedbacks": [
        {
            "date": "By Kao y. on November 16, 2021.",
            "stars": "5 out of 5 stars",
            "text": "It say not to exceed 10lbs total but I did anyway. My bird was 8lbs + the 3lb box = 11lbs. Bird arrived in great condition."
        },
        {
            "date": "By JL on June 9, 2021.",
            "stars": "1 out of 5 stars",
            "text": "How this seller packages its items is not acceptable..."
        }
    ],
    "rating_positive": "89%",
    "feedbacks_percentages": {
        "star_5": "80%",
        "star_4": "9%",
        "star_3": "7%",
        "star_2": "0%",
        "star_1": "5%"
    },
    "products_link": "https://www.amazon.com/s?me=A33W53J5GVPZ8K",
    "buisness_name": "Francis Kunnumpurath",
    "buisness_address": "2612 State Route 80, Lafayette, NY, 13084, US",
    "rating_count_lifetime": 44,
    "country": "US"
}
```

#### Code Example:
다음은 Amazon 판매자 데이터 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = (
        f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}"
    )

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_seller_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "API_TOKEN"
    DATASET_ID = "gd_lhotzucw1etoe5iw1k"

    # Define the dataset with seller URLs
    datasets = [
        {"url": "https://www.amazon.com/sp?seller=A33W53J5GVPZ8K"},
        {"url": "https://www.amazon.com/sp?seller=A33YXLPENB0JBD"},
        {"url": "https://www.amazon.com/sp?seller=A33ZG27WW2U3E6"},
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```

전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_seller_data.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products by Best Sellers
베스트셀러 카테고리의 URL을 제공하여 Amazon에서 가장 많이 판매되는 제품을 확인합니다.

<img width="700" alt="bright-data-web-scraper-api-amazon-best-sellers" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-amazon-best-sellers.png">


#### Key Input Parameters:

| Parameter       | Type     | Description                                    | Required |
|-----------------|----------|------------------------------------------------|----------|
| `category_url`  | `string` | 데이터를 スクレイピング할 베스트셀러 카테고리 URL | Yes      |

#### Performance:
- 입력당 평균 응답 시간: 6분 49초

#### Sample Output Data:
Amazon의 베스트셀러 데이터를 スクレイピング한 후 받게 되는 출력 예시는 다음과 같습니다:

```json
{
    "title": "Amazon Basics Multipurpose Copy Printer Paper, 8.5\" x 11\", 1 Ream, 500 Sheets, White",
    "seller_name": "Amazon.com",
    "brand": "Amazon Basics",
    "initial_price": 9.99,
    "final_price": 7.41,
    "currency": "USD",
    "availability": "In Stock",
    "reviews_count": 178695,
    "rating": 4.8,
    "categories": [
        "Office Products",
        "Paper",
        "Copy & Multipurpose Paper"
    ],
    "asin": "B01FV0F8H8",
    "buybox_seller": "Amazon.com",
    "discount": "-26%",
    "root_bs_rank": 1,
    "url": "https://www.amazon.com/AmazonBasics-Multipurpose-Copy-Printer-Paper/dp/B01FV0F8H8?th=1&psc=1",
    "image_url": "https://m.media-amazon.com/images/I/81x0cTHWQJL._AC_SL1500_.jpg",
    "delivery": [
        "FREE delivery Friday, October 25",
        "Same-Day delivery Today 10 AM - 3 PM"
    ],
    "features": [
        "1 ream (500 sheets) of 8.5 x 11 white copier and printer paper",
        "Works with laser/inkjet printers, copiers, and fax machines",
        "Smooth 20lb weight paper for consistent ink and toner distribution"
    ],
    "bought_past_month": 100000,
    "root_bs_category": "Office Products",
    "bs_category": "Copy & Multipurpose Paper",
    "bs_rank": 1,
    "amazon_choice": true,
    "badge": "Amazon's Choice",
    "seller_url": "https://www.amazon.com/sp?ie=UTF8&seller=ATVPDKIKX0DER&asin=B01FV0F8H8",
    "timestamp": "2024-10-20T13:30:56.666Z"
}
```
#### Code Example:
아래는 Amazon 베스트셀러 데이터 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}&type=discover_new&discover_by=best_sellers_url&limit_per_input=3"

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        time.sleep(10)
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None


def store_data(data, filename="amazon_bestsellers_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "YOUR_API_TOKEN"
    DATASET_ID = "gd_l7q7dkf244hwjntr0"

    datasets = [
        {
            "category_url": "https://www.amazon.com/gp/bestsellers/office-products/ref=pd_zg_ts_office-products"
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```
전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_bestsellers.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products by Category URL
특정 카테고리 URL을 제공하여 Amazon 제품 데이터를 발견하고 수집합니다. 정렬 옵션과 위치 기반 필터로 검색을 사용자 지정할 수 있습니다.

<img width="700" alt="bright-data-web-scraper-api-discover-by-category-url" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-discover-by-category-url.png">

#### Key Input Parameters:
| **Parameter** | **Type**  | **Description**                              | **Required** |
|---------------|-----------|----------------------------------------------|--------------|
| `url`         | `string`  | 제품을 スクレイピング할 카테고리 URL      | Yes          |
| `sort_by`     | `string`  | 제품 결과 정렬 기준      | No           |
| `zipcode`     | `string`  | 위치별 제품 결과를 위한 우편번호| No           |

#### Performance:
- 입력당 평균 응답 시간: 16분 16초

#### Sample Output Data:
지정한 카테고리에서 제품을 スクレイピング한 후 받게 되는 데이터 예시는 다음과 같습니다:
```json
{
    "title": "Quilted Makeup Bag Floral Makeup Bag Cotton Makeup Bag",
    "brand": "WYJ",
    "price": 9.99,
    "currency": "USD",
    "availability": "In Stock",
    "rating": 5,
    "reviews_count": 1,
    "categories": [
        "Beauty & Personal Care",
        "Cosmetic Bags"
    ],
    "asin": "B0DC3WX7RM",
    "seller_name": "yisenshangmaoyouxiangongsi",
    "number_of_sellers": 1,
    "url": "https://www.amazon.com/WYJ-Quilted-Coquette-Aesthetic-Blue/dp/B0DC3WX7RM",
    "image_url": "https://m.media-amazon.com/images/I/71SI04tB6QL._AC_SL1500_.jpg",
    "product_dimensions": "8.7\"L x 2.8\"W x 5.1\"H",
    "item_weight": "2.5 Ounces",
    "variations": [
        {
            "name": "Pink",
            "asin": "B0DC3RKYPF",
            "price": 9.99
        },
        {
            "name": "Blue",
            "asin": "B0DC3WX7RM",
            "price": 9.99
        },
        {
            "name": "Purple",
            "asin": "B0DC47CDDT",
            "price": 9.99
        }
    ],
    "badge": "#1 New Release",
    "top_review": "I love everything about this bag! It's made well and a good size. Super cute!"
}
```

#### Code Example:
아래는 지정한 카테고리 URL에서 제품 수집을 트리거하고 데이터를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}&type=discover_new&discover_by=category_url&limit_per_input=4"

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_bestsellers_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "YOUR_API_TOKEN"
    DATASET_ID = "gd_l7q7dkf244hwjntr0"

    datasets = [
        {
            "url": "https://www.amazon.com/s?i=luggage-intl-ship",
            "sort_by": "Best Sellers",
            "zipcode": "10001",
        },
        {
            "url": "https://www.amazon.com/s?i=baby-products-intl-ship",
            "sort_by": "Avg. Customer Review",
            "zipcode": "",
        },
        {
            "url": "https://www.amazon.com/s?rh=n%3A16225012011&fs=true&ref=lp_16225012011_sar",
            "sort_by": "Price: Low to High",
            "zipcode": "",
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```

전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_discover_by_category_url.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products by Keyword
특정 키워드를 사용하여 제품을 발견합니다.

<img width="700" alt="bright-data-web-scraper-api-discover-by-keyword" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-discover-by-keyword.png">

#### Key Input Parameters:
| **Parameter** | **Type**  | **Description**                   | **Required** |
|---------------|-----------|-----------------------------------|--------------|
| `keyword`     | `string`  | 제품을 검색할 키워드 | Yes          |

#### Performance:
- 입력당 평균 응답 시간: 2분 46초

#### Sample Output Data:
키워드를 사용하여 제품을 검색한 후 받게 되는 출력 예시는 다음과 같습니다:

```json
{
    "title": "SYLVANIA ECO LED Light Bulb, A19 60W Equivalent, 750 Lumens, 2700K, Non-Dimmable, Frosted, Soft White - 8 Count (Pack of 1)",
    "brand": "LEDVANCE",
    "seller_name": "Amazon.com",
    "initial_price": 13.99,
    "final_price": 12.12,
    "currency": "USD",
    "discount": "-13%",
    "rating": 4.7,
    "reviews_count": 48418,
    "availability": "In Stock",
    "url": "https://www.amazon.com/Sylvania-40821-Equivalent-Efficient-Temperature/dp/B08FRSS4BF",
    "image_url": "https://m.media-amazon.com/images/I/81wKhRO66oL._AC_SL1500_.jpg",
    "delivery": [
        "FREE delivery Friday, October 25 on orders shipped by Amazon over $35",
        "Or Prime members get FREE delivery Tomorrow, October 21. Order within 8 hrs 8 mins. Join Prime"
    ],
    "features": [
        "60W Incandescent Replacement Bulb - 750 Lumens",
        "Long-lasting – 7 years lifespan",
        "Energy-saving – Estimated energy cost of $1.08 per year"
    ],
    "discovery_input": {
        "keyword": "light bulb"
    },
    "input": {
        "url": "https://www.amazon.com/Sylvania-40821-Equivalent-Efficient-Temperature/dp/B08FRSS4BF"
    }
}
```
#### Code Example:
아래는 키워드를 기반으로 Amazon 제품 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(
    api_token, dataset_id, datasets, dataset_type="discover_new", discover_by="keyword"
):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}&type={dataset_type}&discover_by={discover_by}"

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = (
        f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}?format=json"
    )

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_keyword_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "API_TOKEN"
    DATASET_ID = "gd_l7q7dkf244hwjntr0"

    # Define the dataset with keywords
    datasets = [{"keyword": "light bulb"}, {"keyword": "dog toys"}]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```

전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_keyword_data.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products Global Dataset
URL을 제공하여 주요 Amazon 도메인 전반에서 제품 데이터를 수집합니다.

<img width="700" alt="bright-data-web-scraper-api-amazon-product-global-dataset" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-amazon-product-global-dataset.png">


#### Key Input Parameters:
| **Parameter** | **Type**  | **Description**           | **Required** |
|---------------|-----------|---------------------------|--------------|
| `url`         | `string`  | Amazon 제품 URL     | Yes          |

#### Performance:
- **입력당 평균 응답 시간**: 1초 미만

#### Sample Output Data:
제품 데이터를 수집한 후 받게 되는 출력 예시는 다음과 같습니다:

```json
{
    "title": "Toys of Wood Oxford Wooden Stacking Rings – Learning to Count – Counting Game with 45 Rings – Wooden Toy for Ages 3 and Above",
    "brand": "Toys of Wood Oxford",
    "seller_name": "Toys of Wood Oxford",
    "initial_price": 23.99,
    "currency": "EUR",
    "final_price": 23.99,
    "availability": "Only 20 left in stock.",
    "rating": 4.5,
    "reviews_count": 1677,
    "asin": "B078TNNZK3",
    "url": "https://www.amazon.de/dp/B078TNNZK3?th=1&psc=1",
    "image_url": "https://m.media-amazon.com/images/I/815t1-d+7BL._AC_SL1500_.jpg",
    "product_dimensions": "43.31 x 11.61 x 11.51 cm; 830 g",
    "categories": [
        "Toys",
        "Baby & Toddler Toys",
        "Early Development & Activity Toys",
        "Sorting, Stacking & Plugging Toys"
    ],
    "delivery": [
        "FREE delivery Friday, 25 October on eligible first order",
        "Or fastest delivery Thursday, 24 October. Order within 4 hrs 40 mins"
    ],
    "features": [
        "Sturdy and stable base plate with 9 pins and 45 beautiful large wooden rings and 10 removable square number plates in rainbow colours.",
        "Great for learning counting, sorting, and matching colors and numbers, as well as practicing simple mathematics.",
        "Made from sustainable wood with eco-friendly and non-toxic paints. Complies with EN71 / CPSA standards."
    ],
    "top_review": "Sehr lehrreich",
    "variations": [
        {
            "name": "Caterpillar Threading Toy",
            "price": 13.99,
            "currency": "EUR"
        },
        {
            "name": "Pack of 15",
            "price": 16.99,
            "currency": "EUR"
        },
        {
            "name": "Pack of 45",
            "price": 23.99,
            "currency": "EUR"
        }
    ],
    "product_rating_object": {
        "one_star": 35,
        "two_star": 0,
        "three_star": 82,
        "four_star": 227,
        "five_star": 1308
    }
}
```
#### Code Example:
아래는 주요 Amazon 도메인 전반에서 제품 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(
    api_token, dataset_id, datasets, dataset_type="trigger", discover_by="url"
):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={
        dataset_id}&type={dataset_type}&discover_by={discover_by}"

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = f"https://api.brightdata.com/datasets/v3/snapshot/{
        snapshot_id}?format=json"

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_products_global_dataset.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "API_TOKEN"
    DATASET_ID = "gd_lwhideng15g8jg63s7"

    # Define the dataset with URLs
    datasets = [
        {"url": "https://www.amazon.com/dp/B0CHHSFMRL/"},
        {
            "url": "https://www.amazon.de/-/en/dp/B078TNNZK3/ref=sspa_dk_browse_2/?_encoding=UTF8&ie=UTF8&sp_csd=d2lkZ2V0TmFtZT1zcF9icm93c2VfdGhlbWF0aWM%3D&pd_rd_w=fHlOu&content-id=amzn1.sym.642a11a6-0e1e-47fa-93c2-5dc9d607a7a1&pf_rd_p=642a11a6-0e1e-47fa-93c2-5dc9d607a7a1&pf_rd_r=4JX920KFM8Q7PR83HJ7V&pd_rd_wg=K1OVN&pd_rd_r=be656f87-1a09-4144-b7cf-4e932d6a73c4&ref_=sspa_dk_browse&th=1"
        },
        {
            "url": "https://www.amazon.co.jp/X-TRAK-Folding-Bicycle-Carbon-Adjustable/dp/B0CWV9YTLV/ref=sr_1_1_sspa?crid=3MKZ2ALHSLFOM&dib=eyJ2IjoiMSJ9.YnBVPwJ7nLxlNGHktwDTFM5v2evnsXlnZTJHJKuG8dLeeRCILpy0Knr3ofiKpUGQYi6xR6y4tgdtal85DJ8u6DD_n9r1oVCXdVo0NFmNAfStU6E-MhBig5p_gZGjluAYv5HgUIoEPl0v3iMiRxZNRfivqB-utxOkPOOfXIBHLemry17XcltUDTQqtJv-kP-ZqdP29mjD2cRlbkALtHPKU44MvBC9WUrNcUHAMrlAxtTAByuriywMqz-w2P0HCeehcZTJ1EiLf2VR8cxCiwuaUbIOU3tr1kDN6D7yYPrgRn4.6AOdSmJsksZkqLg8kNM6EvWxIFOijCsP2zo5NLHn1P4&dib_tag=se&keywords=Bicycles&qid=1716973495&sprefix=%2Caps%2C851&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"
        },
        {
            "url": "https://www.amazon.in/Watches-Women%EF%BC%8CLadies-Stainless-Waterproof-Luminous/dp/B0D31HBWG1/ref=sr_1_2_sspa?dib=eyJ2IjoiMSJ9.1zFa2vTCZdD-bv6Knt_pWqvcRZPSSTPDwgMClRJNsWqdyGdCmryjEAfWpd-ZhwhC3vvNx9A0G2Gt1R952e7huzlukge2bmJETNf-kHBoWS5kV6g0pUVapEyDOEAGcw5ZvWlkeuLQ9oIwuhckRC6ARCt2yglYV-1HpP7lVGXotK6K6tjrdKxUSAOZJSXeOGP3dGuYPTjo9sllOrwA7FC2GG00aDcsSTzURENFj1c2rS-vNHkYmxOL1JYuwDWK2PJdMpsmkJw3jeMdgaiw7jG5ppMfAjwiETVldQzhHGVUFV8.manfNZwtTUhvDuSGdh32APM1_SmnNiKgOGabyA7rXBo&dib_tag=se&qid=1716973272&rnid=2563505031&s=watch&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGZfYnJvd3Nl&psc=1"
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```

전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_products_global_dataset.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products Global Dataset - Discover by Category URL
특정 카테고리 URL을 제공하여 제품을 발견합니다.

<img width="700" alt="bright-data-web-scraper-api-amazon-product-global-category-url" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-amazon-product-global-category-url.png">


#### Key Input Parameters:
| **Parameter** | **Type** | **Description**                               | **Required** |
|---------------|----------|-----------------------------------------------|--------------|
| `url`         | `string` | 제품을 スクレイピング할 카테고리 URL | Yes          |
| `sort_by`     | `string` |Criteria for sorting the results               | No           |
| `zipcode`     | `string` | 위치별 결과를 위한 우편번호         | No           |

#### Performance:
- 입력당 평균 응답 시간: 3분 57초

#### Sample Output Data:
제품 데이터를 수집한 후 받게 되는 출력 예시는 다음과 같습니다:
```json
{
    "title": "De'Longhi Stilosa EC230.BK, Traditional Barista Pump Espresso Machine, Espresso and Cappuccino, 2 cups, Black",
    "brand": "De'Longhi",
    "seller_name": "Hughes Electrical",
    "initial_price": 104.99,
    "final_price": 94,
    "currency": "GBP",
    "availability": "Only 1 left in stock.",
    "rating": 3.9,
    "reviews_count": 395,
    "asin": "B085J8LV4F",
    "url": "https://www.amazon.co.uk/dp/B085J8LV4F?th=1&psc=1",
    "image_url": "https://m.media-amazon.com/images/I/715gqhkOEiL._AC_SL1500_.jpg",
    "categories": [
        "Cooking & Dining",
        "Coffee, Tea & Espresso",
        "Coffee Machines",
        "Espresso & Cappuccino Machines"
    ],
    "delivery": [
        "FREE delivery 25 - 28 October",
        "Or fastest delivery Tomorrow, 22 October. Order within 3 hrs 59 mins"
    ],
    "features": [
        "Unleash your inner barista and create all your coffee shop favourites at home",
        "15-bar pump espresso maker with a stainless steel boiler for perfect coffee extraction",
        "Steam arm to create frothy cappuccinos and smooth lattes",
        "Combination of matt and glossy black finish with an anti-drip system"
    ],
    "input": {
        "url": "https://www.amazon.co.uk/DeLonghi-EC230-BK-Traditional-Espresso-Cappuccino/dp/B085J8LV4F/ref=sr_1_4"
    },
    "discovery_input": {
        "url": "https://www.amazon.co.uk/b/?_encoding=UTF8&node=10706951&ref_=Oct_d_odnav_d_13528598031_1",
        "sort_by": "Best Sellers",
        "zipcode": ""
    }
}
```
#### Code Example:
아래는 카테고리 URL로 제품 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(api_token, dataset_id, datasets, dataset_type="discover_new", discover_by="category_url", limit_per_input=4):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={dataset_id}&type={
        dataset_type}&discover_by={discover_by}&limit_per_input={limit_per_input}"

    # Sending API request to trigger dataset collection
    response = requests.post(
        trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = f"https://api.brightdata.com/datasets/v3/snapshot/{
        snapshot_id}?format=json"

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_category_url_data.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "API_TOKEN"
    DATASET_ID = "gd_lwhideng15g8jg63s7"

    # Define the dataset with category URLs, sort_by, and zipcodes
    datasets = [
        {"url": "https://www.amazon.com/s?i=luggage-intl-ship",
            "sort_by": "Featured", "zipcode": "10001"},
        {"url": "https://www.amazon.de/-/en/b/?node=1981001031&ref_=Oct_d_odnav_d_355007011_2&pd_rd_w=OjE3S&content-id=amzn1.sym.0069bc39-a323-47d6-a8fb-7558e4a563e4&pf_rd_p=0069bc39-a323-47d6-a8fb-7558e4a563e4&pf_rd_r=6YXZ7HGFNNEAF0GSDPDH&pd_rd_wg=0yR1G&pd_rd_r=a95cb46c-78ef-4b7b-845d-49fe04556440", "sort_by": "Price: Low to High", "zipcode": ""},
        {"url": "https://www.amazon.co.uk/b/?_encoding=UTF8&node=10706951&bbn=11052681&ref_=Oct_d_odnav_d_13528598031_1&pd_rd_w=LghVp&content-id=amzn1.sym.7414f21e-2c95-4394-9a75-8c1b3641bcea&pf_rd_p=7414f21e-2c95-4394-9a75-8c1b3641bcea&pf_rd_r=EE0PQWMSY2J0G8M032EB&pd_rd_wg=7snrU&pd_rd_r=349e1e79-8bf8-4e00-947d-17eab2942b8d", "sort_by": "Best Sellers", "zipcode": ""},
        {"url": "https://www.amazon.co.jp/-/en/b/?node=377403011&ref_=Oct_d_odnav_d_15314601_0&pd_rd_w=ajUV4&content-id=amzn1.sym.0d505cca-fde9-497c-b5f8-e827c26fad17&pf_rd_p=0d505cca-fde9-497c-b5f8-e827c26fad17&pf_rd_r=92HSETNKKN3RTA615BV7&pd_rd_wg=AwOOk&pd_rd_r=629211d8-6768-478c-94a2-829a0a0ca2a6", "sort_by": "", "zipcode": ""}
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```
전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_product_global_category_url.json)을 다운로드하여 확인할 수 있습니다.

### Amazon Products Global Dataset - Discover by Keywords
Amazon 도메인 전반에서 특정 키워드를 사용하여 제품을 발견합니다.

<img width="700" alt="bright-data-web-scraper-api-amazon_global_dataset_by_keyword" src="https://github.com/bright-kr/Amazon-scraper/blob/main/images/bright-data-web-scraper-api-amazon_global_dataset_by_keyword.png">

#### Key Input Parameters:
| **Parameter**      | **Type**   | **Description**                            | **Required** |
|--------------------|------------|--------------------------------------------|--------------|
| `keywords`         | `string`   | 제품을 검색할 키워드         | Yes          |
| `domain`           | `string`   | 검색할 Amazon 도메인             | Yes          |
| `pages_to_search`  | `number`   | 검색할 페이지 수                  | No           |

#### Performance:
- 입력당 평균 응답 시간: 56초

#### Sample Output Data:
키워드 검색으로 제품을 검색한 후 받게 되는 출력 예시는 다음과 같습니다:
```json
{
    "title": "Mitutoyo 500-197-30 Electronic Digital Caliper AOS Absolute Scale Digital Caliper, 0 to 8\"/0 to 200mm Measuring Range, 0.0005\"/0.01mm Resolution",
    "brand": "Mitutoyo",
    "seller_name": "Everly Home & Gift",
    "initial_price": 157.97,
    "final_price": 137.77,
    "currency": "USD",
    "availability": "In Stock",
    "rating": 4.8,
    "reviews_count": 88,
    "asin": "B01N6C3EGR",
    "url": "https://www.amazon.com/dp/B01N6C3EGR?th=1&psc=1",
    "image_url": "https://m.media-amazon.com/images/I/61Gigoh3LbL._SL1500_.jpg",
    "categories": [
        "Industrial & Scientific",
        "Test, Measure & Inspect",
        "Dimensional Measurement",
        "Calipers",
        "Digital Calipers"
    ],
    "delivery": [
        "FREE delivery Saturday, October 26",
        "Or Prime members get FREE delivery Tomorrow, October 22"
    ],
    "features": [
        "Hardened stainless steel construction for protection of caliper components",
        "Digital, single-value readout LCD display in metric units for readability",
        "Measuring Range 0 to 8\"/0 to 200mm",
        "Measurement Accuracy +/-0.001",
        "Resolution 0.0005\"/0.01mm"
    ],
    "input": {
        "url": "https://www.amazon.com/Mitutoyo-500-197-30-Electronic-Measuring-Resolution/dp/B01N6C3EGR"
    },
    "discovery_input": {
        "keywords": "Mitutoyo",
        "domain": "https://www.amazon.com",
        "pages_to_search": 1
    }
}
```
#### Code Example:
아래는 키워드 검색으로 제품 수집을 트리거하고 결과를 JSON 파일에 저장하는 Python 스크립트입니다:
```python
import json
import requests
import time


def trigger_datasets(
    api_token, dataset_id, datasets, dataset_type="discover_new", discover_by="keywords"
):
    headers = {
        "Authorization": f"Bearer {api_token}",
        "Content-Type": "application/json",
    }

    trigger_url = f"https://api.brightdata.com/datasets/v3/trigger?dataset_id={
        dataset_id}&type={dataset_type}&discover_by={discover_by}"

    # Sending API request to trigger dataset collection
    response = requests.post(trigger_url, headers=headers, data=json.dumps(datasets))

    if response.status_code == 200:
        print("Data collection triggered successfully!")
        snapshot_id = response.json().get("snapshot_id")
        return snapshot_id if snapshot_id else print("No snapshot ID returned.")
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None


def get_snapshot_data(api_token, snapshot_id):
    headers = {"Authorization": f"Bearer {api_token}"}
    snapshot_url = f"https://api.brightdata.com/datasets/v3/snapshot/{
        snapshot_id}?format=json"

    # Polling until the snapshot data is ready
    while True:
        response = requests.get(snapshot_url, headers=headers)

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            print("Snapshot still processing... retrying.")
        else:
            print(f"Error: {response.status_code} - {response.text}")
            return None
        time.sleep(10)


def store_data(data, filename="amazon_global_dataset_by_keyword.json"):
    if data:
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Data saved in {filename}.")
    else:
        print("No data to store.")


if __name__ == "__main__":
    API_TOKEN = "YOUR_API_TOKEN"
    DATASET_ID = "gd_lwhideng15g8jg63s7"

    # Define the dataset with keywords, domain, and pages_to_search
    datasets = [
        {
            "keywords": "Mitutoyo",
            "domain": "https://www.amazon.com",
            "pages_to_search": 1,
        },
        {
            "keywords": "smart watch",
            "domain": "https://www.amazon.co.uk",
            "pages_to_search": 2,
        },
        {
            "keywords": "football",
            "domain": "https://www.amazon.in",
            "pages_to_search": 4,
        },
        {
            "keywords": "baby cloth",
            "domain": "https://www.amazon.de",
            "pages_to_search": 3,
        },
    ]

    # Trigger dataset collection
    snapshot_id = trigger_datasets(API_TOKEN, DATASET_ID, datasets)

    if snapshot_id:
        # Retrieve the data once the snapshot is ready
        data = get_snapshot_data(API_TOKEN, snapshot_id)
        if data:
            store_data(data)
```
전체 출력은 [이 샘플 JSON 파일](https://github.com/bright-kr/Amazon-scraper/blob/main/output_data/amazon_global_dataset_by_keyword.json)을 다운로드하여 확인할 수 있습니다.