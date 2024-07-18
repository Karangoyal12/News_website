# Daily News Website

![Daily News Logo](daily_news.jpg)

## Introduction
Daily News Website is a web application that allows users to search and browse news articles from various categories like Technology, Finance, and Politics. It uses the News API to fetch and display the latest news. This project demonstrates my skills in web development using HTML, CSS, and JavaScript.

## Features
- **Search Functionality**: Users can search for news articles based on keywords.
- **Category Browsing**: Users can browse news articles by categories such as Technology, Finance, and Politics.
- **Responsive Design**: The website is designed to be responsive, ensuring a seamless experience on both desktop and mobile devices.
- **Dynamic Content**: News articles are dynamically loaded and displayed using JavaScript.

## Technologies Used
- **HTML5**: For structuring the web page.
- **CSS3**: For styling the web page.
- **JavaScript (ES6+)**: For fetching data from the API and dynamically updating the content.
- **News API**: For retrieving the latest news articles.

## Setup Instructions
To view and run this project locally, follow these steps:

1. **Clone the repository**:
    ```sh
    git clone https://github.com/yourusername/daily-news-website.git
    cd daily-news-website
    ```

2. **Open `index.html`** in your preferred web browser.

## Usage
- The homepage loads with the latest news articles from India by default.
- Use the search bar to find news articles by keywords.
- Click on category links (Technology, Finance, Politics) to filter news articles by category.
- Click on any news card to open the full article in a new tab.



## API Usage
This project uses the News API to fetch news articles. Ensure you have a valid API key from [News API](https://newsapi.org/).

### JavaScript Code (script.js)
```javascript
const API_KEY = "YOUR_NEWS_API_KEY";
const url = "https://newsapi.org/v2/everything?q=";

window.addEventListener('load',() => fetchNews("India"));

function reload() {
    window.location.reload();
}

async function fetchNews(query, page = 1, pageSize = 20) {
    const res = await fetch(`${url}${query}&sortBy=publishedAt&page=${page}&pageSize=${pageSize}&apiKey=${API_KEY}`);
    const data = await res.json();
    console.log(data);
    bindData(data.articles);
}

function bindData(articles) {
    const cardsContainer = document.getElementById('cards-container');
    const newsCardTemplate = document.getElementById('template-news-card');

    cardsContainer.innerHTML = '';

    articles.forEach(article => {
        if (!article.urlToImage) return;
        const cardClone = newsCardTemplate.content.cloneNode(true);
        fillDataInCard(cardClone, article);
        cardsContainer.appendChild(cardClone);
    });
}

function fillDataInCard(cardClone, article) {
    const newsImg = cardClone.querySelector("#news-img");
    const newsTitle = cardClone.querySelector("#news-title");
    const newsSource = cardClone.querySelector("#news-source");
    const newsDesc = cardClone.querySelector('#news-desc');

    newsImg.src = article.urlToImage;
    newsTitle.innerHTML = article.title;
    newsDesc.innerHTML = article.description;

    const date = new Date(article.publishedAt).toLocaleString("en-US", { timeZone: "Asia/Jakarta" });

    newsSource.innerHTML = `${article.source.name} · ${date}`;

    cardClone.firstElementChild.addEventListener("click", () => {
        window.open(article.url, "_blank");
    });
}

let curSelectedNav = null;
function onNavItemClick(id) {
    fetchNews(id);
    const navItem = document.getElementById(id);
    curSelectedNav?.classList.remove("active");
    curSelectedNav = navItem;
    curSelectedNav.classList.add("active");
}

const searchButton = document.getElementById("search-button");
const searchText = document.getElementById("search-text");
searchButton.addEventListener("click", () => {
    const query = searchText.value;
    if (!query) return;
    fetchNews(query);
    curSelectedNav?.classList.remove("active");
    curSelectedNav = null;
});

searchText.addEventListener('keydown', (event) => {
    if (event.key == 'Enter') {
        event.preventDefault();
        const query = searchText.value.trim();
        if (query) {
            fetchNews(query);
            curSelectedNav?.classList.remove("active");
            curSelectedNav = null;
            searchText.blur();
        }
    }
});


