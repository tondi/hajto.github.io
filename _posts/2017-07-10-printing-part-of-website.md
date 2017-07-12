---
layout: post
title: "Printing part of website a.k.a. how I got my CV printed"
status: released
type: post
comments: true
list: general
set: general
---

I've been looking for a new job for quite some time. Sooner or later I had to make a resume or a CV. I was looking for `Free CV generator` google poped a bunch of results. But guess how many of them were actually free...

<!--more-->

Of course none. After experiencing six hours of false advertising I decided that's enough. If I can see something on a website I surely can print it, huh? After few minutes of googling it up I found this <a href="https://stackoverflow.com/questions/12997123/print-specific-part-of-webpage"> SO post</a>.

Basically `@pmtamal` approach is to copy all interesting contents in to new Window and invoke `Print` there.

{% highlight javascript %}
var prtContent = document.getElementById("your div id");
var WinPrint = window.open('', '', 'left=0,top=0,width=800,height=900,toolbar=0,scrollbars=0,status=0');
WinPrint.document.write(prtContent.innerHTML);
WinPrint.document.close();
WinPrint.focus();
WinPrint.print(); //I would rather prefer call ctrl+P manually
{% endhighlight%}

It does work, but it is a bit flawed. It does not copy styles... Let's apply some basic JS knowledge.

{% highlight javascript %}
var styles = document.getElementsByTagName('style')
var links = document.getElementsByTagName('link')
{% endhighlight%}

And we only have to shove it into that window we created.

{% highlight javascript %}
function PrintDivById(id) {
  var prtContent = document.getElementById(id);
  console.log(prtContent.innerHTML);
  var WinPrint = window.open('', '', 'left=0,top=0,width=800,height=900,toolbar=0,scrollbars=0,status=0');
  var document_html = "<html> <head>";
  var styles = document.getElementsByTagName('style')
  for(var i =0; i < styles.length; i++){
   	document_html += styles[i].outerHTML;
  }
  var links = document.getElementsByTagName("link");
  for(var i = 0; i < links.length; i++){
    document_html +=links[i].outerHTML;
  }
  document_html += "</head> <body>";
  document_html += prtContent.outerHTML;
  document_html += "</body> </html>";
  WinPrint.document.write(document_html);
  WinPrint.document.close();
  WinPrint.focus();
}
{% endhighlight%}

And in a bit more functional way..

{% highlight javascript %}
//By Baransu
function printDivById(id) {
  const prtContent = document.getElementById(id);
  const windowPrint = window.open("", "", "left=0,top=0,width=800,height=900,toolbar=0,scrollbars=0,status=0");
  const documentHtml = [
    "<html><head>",
    document.getElementsByTagName("style").map(style => style.outerHTML).join(""),
    document.getElementsByTagName("link").map(link => link.outerHTML).join(""),
    "</head><body>",
    prtContent.outerHTML,
    "</body></html>"
  ].join("")
  windowPrint.document.write(document_html);
  windowPrint.document.close();
  windowPrint.focus();
}
{% endhighlight%}
