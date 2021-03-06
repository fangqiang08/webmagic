![logo](https://raw.github.com/code4craft/webmagic/master/asserts/logo.jpg)

[Readme in Chinese](https://github.com/code4craft/webmagic/tree/master/zh_docs)

[User Manual (Chinese)](https://github.com/code4craft/webmagic/blob/master/user-manual.md)


[![Build Status](https://travis-ci.org/code4craft/webmagic.png?branch=master)](https://travis-ci.org/code4craft/webmagic)

>A scalable crawler framework. It covers the whole lifecycle of crawler: downloading, url management, content extraction and persistent. It can simplify the development of a  specific crawler.

## Features:

* Simple core with high flexibility.
* Simple API for html extracting.
* Annotation with POJO to customize a crawler, no configuration.
* Multi-thread and Distribution support.
* Easy to be integrated.

## Install:
  
Add dependencies to your pom.xml:

		<dependency>
            <groupId>us.codecraft</groupId>
            <artifactId>webmagic-core</artifactId>
            <version>0.4.2</version>
        </dependency>
		<dependency>
            <groupId>us.codecraft</groupId>
            <artifactId>webmagic-extension</artifactId>
            <version>0.4.2</version>
        </dependency>

## Get Started:

### First crawler:

Write a class implements PageProcessor：

```java
    public class OschinaBlogPageProcesser implements PageProcessor {

        private Site site = Site.me().setDomain("my.oschina.net");

        @Override
        public void process(Page page) {
            List<String> links = page.getHtml().links().regex("http://my\\.oschina\\.net/flashsword/blog/\\d+").all();
            page.addTargetRequests(links);
            page.putField("title", page.getHtml().xpath("//div[@class='BlogEntity']/div[@class='BlogTitle']/h1").toString());
            page.putField("content", page.getHtml().$("div.content").toString());
            page.putField("tags",page.getHtml().xpath("//div[@class='BlogTags']/a/text()").all());
        }

        @Override
        public Site getSite() {
            return site;

        }

        public static void main(String[] args) {
            Spider.create(new OschinaBlogPageProcesser()).addUrl("http://my.oschina.net/flashsword/blog")
                 .addPipeline(new ConsolePipeline()).run();
        }
    }
```

* `page.addTargetRequests(links)`
	
	Add urls for crawling.
    
You can also use annotation way:

```java
	@TargetUrl("http://my.oschina.net/flashsword/blog/\\d+")
	public class OschinaBlog {

	    @ExtractBy("//title")
	    private String title;

	    @ExtractBy(value = "div.BlogContent",type = ExtractBy.Type.Css)
	    private String content;

	    @ExtractBy(value = "//div[@class='BlogTags']/a/text()", multi = true)
	    private List<String> tags;

	    public static void main(String[] args) {
	        OOSpider.create(
	        	Site.me(),
				new ConsolePageModelPipeline(), OschinaBlog.class).addUrl("http://my.oschina.net/flashsword/blog").run();
	    }
	}
```
		
### Docs and samples:

The architecture of webmagic (refered to [Scrapy](http://scrapy.org/))

![image](http://code4craft.github.io/images/posts/webmagic.png)

Javadocs: [http://code4craft.github.io/webmagic/docs/en/](http://code4craft.github.io/webmagic/docs/en/)

There are some samples in `webmagic-samples` package.

### Lisence:

Lisenced under [Apache 2.0 lisence](http://opensource.org/licenses/Apache-2.0)

### Contributors:

Thanks these people for commiting source code, reporting bugs or suggesting for new feature:

* [yuany](https://github.com/yuany)
* [yxssfxwzy](https://github.com/yxssfxwzy)
* [linkerlin](https://github.com/linkerlin)
* [d0ngw](https://github.com/d0ngw)
* [xuchaoo](https://github.com/xuchaoo)
* [supermicah](https://github.com/supermicah)
* [SimpleExpress](https://github.com/SimpleExpress)
* [aruanruan](https://github.com/aruanruan)
* [l1z2g9](https://github.com/l1z2g9)
* [zhegexiaohuozi](https://github.com/zhegexiaohuozi)
* [ywooer](https://github.com/ywooer)
* [yyw258520](https://github.com/yyw258520)
* [perfecking](https://github.com/perfecking)
* [lidongyang](http://my.oschina.net/lidongyang)


### Thanks:

To write webmagic, I refered to the projects below :

* **Scrapy**

	A crawler framework in Python.
 
	[http://scrapy.org/](http://scrapy.org/)

* **Spiderman**

	Another crawler framework in Java.
	
	[https://gitcafe.com/laiweiwei/Spiderman](https://gitcafe.com/laiweiwei/Spiderman)

### Mail-list:

[https://groups.google.com/forum/#!forum/webmagic-java](https://groups.google.com/forum/#!forum/webmagic-java)


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/code4craft/webmagic/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

