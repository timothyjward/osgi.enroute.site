---
title: " "  
summary: Provides an introduction into OSGi and explains how OSGi enRoute uses OSGi
noindex: true
layout: book
---

The OSGi Alliance, since its foundation in 1998, has pursued the goal of creating the open software specifications necessary for creating the highly maintainable, agile and so economically sustainable modular foundations essential for tomorrow's software eco-systems. Due to the on-going efforts of many OSGi Alliance members, todays OSGi Alliance Specifications achieve these objectives.

Now, leverage these foundations, enRoute has been created to provide a simple developer on-ramp for those unfamiliar OSGi™. 

enRoute provides an OSGi/Java development environment that aims to be as simple to use as other popular rapid delivery frameworks for simple applications; but critically enRoute achieves this without sacrificing the maintainability, adaptiability & evolvability advantages unique to OSGi/Java. enRoute leverages the latest OSGi Alliance Specifications, to provide a simple set of `Best Practice` Tutorials and associated Examples. At least initially, enRoute takes an opinionated approach the Development toolchain focusing on Maven.

`I hear and I forget. I see and I remember. I do and I understand!` 

Confucius - Chinese philosopher & reformer (551 BC - 479 BC)

So lets get started!
 
<a href="/tutorial/020-tutorial_qs.html" 
onMouseOver="return changeImage()"
onMouseOut= "return changeImageBack()"
onMouseDown="return handleMDown()"
onMouseUp="return handleMUp()"
><img
name="jsbutton" src="enroute-logo-64.png" width="110" height="110" border="0"
alt="javascript button"></a>
 
<script language="JavaScript">
 
upImage = new Image();
upImage.src = "enroute-logo-64.png";
downImage = new Image();
downImage.src = "enroute-logo-64.png"
normalImage = new Image();
normalImage.src = "enroute-logo-64.png";
 
function changeImage()
{
  document.images["jsbutton"].src= upImage.src;
  return true;
}
function changeImageBack()
{
   document.images["jsbutton"].src = normalImage.src;
   return true;
}
function handleMDown()
{
 document.images["jsbutton"].src = downImage.src;
 return true;
}
function handleMUp()
{
 changeImage();
 return true;
}
</script>


