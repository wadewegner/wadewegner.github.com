---
author: admin
comments: true
date: 2007-04-26 17:22:18+00:00
layout: post
slug: biztalk-2006-xpath-expression-gotcha
title: BizTalk 2006 XPath expression gotcha!
wordpress_id: 93
categories:
- BizTalk Server
- XPath
---

I built an XPath expression to extract the contents of an XML element. I tested this XPath in a utility I created that executes the expression, and writes the results to the file system. The XPath expression worked perfectly, and returned the contents of the XML element. Here's a quick and dirty example ...

My XML file:

	<ns0:Example xmlns:ns0="[http://example/](http://example/)">  
		<ns0:Contents>1169861</ns0:Contents>  
	</ns0:Example>

And my XPath expression:

	/*[local-name()='Example' and namespace-uri()='http://example/']/*[local-name()='Contents' and namespace-uri()='http://example/']/text()

(Don't forget the "/text()" at the end, or you'll get the entire element!)

And, using my utility, I get the following (expected!) output:

	1169861

However, when run in BizTalk 2006, I got that ever-so-annoyingly-generic error message:

	Object reference not set to an instance of an object.

I confirmed that this error occured when I set my orchestration's local numeric variable equal to the the following expression in my BizTalk orchestration:

	xPathExpression = "/*[local-name()='Example' and namespace-uri()='http://example/']/*[local-name()='Contents' and namespace-uri()='http://example/']/text()";
 
	contents = xpath(msgExample, xPathExpression);

What gives? The XPath expression works perfectly in my utility, but it doesn't in a BizTalk orchestration? So, I decided see if I could find any help on Google. No joke, I searched for "biztalk xpath gotcha" and found [Aaron Saikovski's](http://ruskydotnet.blogspot.com/) blog detailing [this problem](http://ruskydotnet.blogspot.com/2006/04/biztalk-2004-xpath-expression-gotcha.html). His note indicated that, in BizTalk 2004, you had to wrap your XPath expressing with string()! It's that simple. So, changing my XPath to the following ...

	xPathExpression =
		"string(/*[local-name()='Example' and namespace-uri()='http://example/']/*[local-name()='Contents' and namespace-uri()='http://example/']/text())";

... saved the day!

What a fun quirk! Thanks Aaron!