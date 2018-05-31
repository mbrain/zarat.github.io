---
layout: post
title: Responsive Webdesign
author: Manuel Zarat
category: webdesign
tags: homepage programm
published: true
---

<span>
  {% for tag in page.tags %}
    {% capture tag_name %}{{ tag }}{% endcapture %}
    <a href="/tag/{{ tag_name }}"><code class="highligher-rouge"><nobr>{{ tag_name }}</nobr></code>&nbsp;</a>
  {% endfor %}
</span>

Beim Responsive Webdesign wird das Layout einer Website mit Hilfe von HTML und CSS so gestaltet, dass es auf Desktop, Tablet und Smartphone eine optimale Benutzerfreundlichkeit bietet.

Der Begriff Responsive Webdesign bedeutet soviel wie „reagierendes Webdesign“. Elemente sowie der strukturelle Aufbau der Website passen sich dabei interaktiv der Bildschirmauflösung des jeweiligen Endgeräts an.

<!--excerpt_separator-->

Hier gilt es bereits in der konzeptionellen Ausarbeitungsphase die inhaltliche Struktur für die verschiedenen Auflösungen zu bestimmen. Unter Umständen sollen nicht alle Inhalte, die auf dem Desktop angezeigt werden, auch auf dem Smartphones zu sehen sein. Denken Sie an die verfügbare Bandbreite und das teilweise limitierte Datenvolumen von mobilen Endgeräten. Darauf sollte bei der Erstellung von Beginn an geachtet werden, um die Datenlast so gering wie möglich zu halten.

Für Smartphones gelten dabei 320 bis 480px, für Tablets 768 bis 1024px und Desktops besitzen in der Regel eine Auflösung von 1024px+

Als Media Query (Medienabfrage) wird die Abfrage einer Liste von Kriterien bezeichnet, die ein Ausgabemedium erfüllen muss, damit ein Stylesheet zur Verarbeitung eingebunden wird. Medienabfragen bestehen aus einem Medientyp (z.B. Desktop, Tablet oder Handy), einem Medienmerkmal (z.B. Farbfähigkeit, Auflösung, Format) oder beidem.

@media definiert innerhalb des style-Tags oder in einer CSS Datei Formatdefinitionen für ein Ausgabemedium bestimmter Größe.

<pre>@media screen and (min-width : 320px) { 
  div.demo {
    background: green; 
  }
}</pre>

Dabei kann auch ein Bereich zwischen 2 Werten festgelegt werden.

<pre>@media screen and (min-width: 320px) and (max-width: 480px) { 
  div.demo { 
    background: red; 
  } 
}</pre>

Beim Einbinden von Produktbildern sollte unbedingt auf die Auflösung des jeweiligen Endgerätes geachtet werden um bestmögliches Aussehen zu Erzielen.

<pre>@media screen and (min-resolution: 192dpi) { 
  div.demo { 
    background-image: url("example.com/images/hd.jpg"); 
  } 
}</pre>

Um dem Interpreter mitzuteilen welche Regeln angewendet werden, werden besagte Media Queries genutzt.

<pre><he<!--strip -->ad>
  <me<!--strip -->ta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no" />
  <li<!--strip -->nk rel="stylesheet" href="style.css" />
</he<!--strip -->ad></pre>

Es gibt jedoch auch die Möglichkeit die Regeln bereits im HTML Header festzulegen. Bei großen Stylesheets wo 3 verschiedene Darstellungsoptionen definiert werden, spart es Resourcen, die 3 Styles in eigene Dateien zu verfrachten und je nach Anforderung nur die jeweilige Datei zu laden.

<pre><li<!--strip -->nk rel='stylesheet' media='screen and (min-width: 320px) and (max-width: 460px)' href='special.css' /></pre>

Studien zum Absatz mobiler Geräte belegen eine stetige Zunahme bei Tablets und Smartphones. Fast täglich werden neue Entwicklungen und Technologien präsentiert und auch die Fachpresse überschlägt sich regelrecht. Sollten Sie gerade vor einem Desktop sitzen, können Sie die Größe des Browserfensters ändern und dabei beobachten, wie die Website darauf reagiert.

Die Navigation beim responsive Webdesign ist meist eine der größten Herausforderungen für Webdesigner und Entwickler. Immerhin soll sie sowohl auf einen großen Monitor, als auch auf einem Smartphone ordentlich und benutzerfreundlich gelöst sein.

Zum einen muss man den jeweils zur Verfügung stehenden Platz möglichst geschickt einteilen. Zum Anderen jedoch auch die Navigation mit einer Maus oder der Bedienung eines Touchscreens berücksichtigen. Zwei völlig verschiedene Ansätze also. Da die Anforderungen an ein Menü bei jedem Projekt andere sind, gibt es wohl keine allgemeingültige Lösung. Es ist auch wichtig, eine Lösung zu finden, die bei einer unterschiedlichen Anzahl von Menüpunkten funktioniert.

Im Responsive Webdesign wird natürlich auch die optimale Skalierung und Auslieferung von Bildern ganz groß geschrieben. Javascript bietet eine tolle und flexible Möglichkeit, Bilder dynamisch an die vorhandene Bildschirmauflösung anzupassen und für das Gerät optimiert auszuliefern.
