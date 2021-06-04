## What is Scraping and Why Use it?

scraping is a technique used to grab data out of the HTML that makes up a web page.

##  HTML Using Nokogiri and Open-URI

[Open-URI](https://ruby-doc.org/stdlib-2.6.3/libdoc/open-uri/rdoc/OpenURI.html) is a module in Ruby that allows us to programmatically make HTTP requests. It gives us a bunch of useful methods 

```
html = open("https://flatironschool.com/")
```

[^This sets a var: html, so that we can use the url in nokogiri]: 



```
doc = Nokogiri::HTML(html)
```

[^now we set a var: doc, and now we can open IRB and access all the html "docs" from the url]: 

Nokogiri's `.css` method can be called on the `doc` variable  like so.

```
doc.css(".headline-26OIBN")
```

