# Search Engine

Search Engine is a web-based application or software that allows users to input keywords and queries to retrieve some information from internet.

## Basic Search Engine

> Implementation is based on [this article](https://voskan.host/2023/08/04/building-search-engine-in-golang/)

To create a very very basic kind of Search Engine, you need to have at least these component.

1. **Crawler**
2. **Indexer**
3. **Search Interface**

### Crawler

It crawls the web pages and their absolute links to recursively visit the connected web pages and retrieve the data from them.

- Using concurrency can be helpful.
- Consider a cool-down or delay between crawling the websites.
- Having a more efficient algorithm for crawling means that you can better be updated.

### Indexer

It create indices for new words and add save related URLs for this word or index. In this basic search engine example it does not matter which kind of database you are using as long as you can retrieve URLs by the indices later.

- It's important to keep track of the data structure of the indices and data you stored. As you index more pages, you will have more and more data to access.

### Search Interface

It receives the user input or query, and should decide on which URLs to return to the user. So here we can use the URLs and indices that we have stored. This component can have different kind of algorithms for handling the request.

- The performance of the Search Engine plays an important role. It should provide a balance between being speed and accuracy. 

## Characteristics of an Advanced Search Engines

The components mentioned in 'Creating a Basic Search Engine' is not enough to have an advance search engine system.

- We need a **Ranking Algorithm** to determine the relevance of the web page.
- Features such as **Filters, Suggestions, Advanced search options, Promoting a advertise, Personalization, and User Feedback** can be added to the Search Engine.
- **Query Parsing** is very critical to have a good accuracy. For instance, identifying and applying keywords like "and" or "or". **Text Processing** and **Handling Typos** are very useful tools.
- It is crucial to use or create the right **Database** option.
- Using **Cache** in the Search Engine can effectively enhance the performance of the system.
- It is also important to protect users against spam, malware or any other kind of **Security** threats.


# See also

What is BM25 or TF-IDF?