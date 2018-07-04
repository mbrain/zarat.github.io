---
layout: post
title: Taxonomien und Terme
author: Manuel Zarat
category: tutorials
tags: protokoll programm
permalink: /blog/taxonomien-und-terme
---

<p>Relationen sollen Inhalte nach vorgegebenen Merkmalen gruppieren. Bereits eingebaute Relationen sind Inhaltstypen (type) und Kategorien (category), die standardmäßig beim Erstellen neuer Inhalte vergeben werden können. In weiterer Folge ist (statt bisher nur flach) eine Aufteilung in flache und hierarchische Taxonomien geplant. Derzeit funktionieren Taxonomien rein flach: einem Inhalt können mehrere Taxonomien vergeben werden, die wiederum jeweils mehrere Terme referenzieren können. In der Datenbank spielt sich das in 3 (nimmt man Item dazu dann 4) Tabellen ab.</p>
<!--excerpt_separator-->
<h3>Tabelle: term</h3>
<p>In dieser Tabelle werden - wie der Name mehr oder weniger verrät - einfach nur die Terme definiert. Vielleicht bekommen sie in Zukunft weitere Attribute, im Moment nicht. Ich nehme als Beispiel</p>
<ul>
<li>post</li>
<li>page</li>
<li>Allgemeines</li>
<li>Spezielles</li>
<li>Wichtig</li>
</ul>
<h3> </h3>
<h3>Tabelle: term_taxonomy</h3>
<p>Hier wird die Taxonomie (Kategorie, Stichwort,...) eines Terms beschrieben. Beispiel</p>
<ul>
<li>type</li>
<li>category</li>
<li>tag</li>
</ul>
<h3> </h3>
<h3>Tabelle: term_relation</h3>
<p>Hier werden die Terme schließlich mit den Items referenziert. Also welche object_id welche term_id als relation_id hat. Klingt jetzt vielleicht etwas verworren macht das ganze aber sehr dynamisch.</p>
<ul>
<li>Item 1: type =&gt; page</li>
<li>Item 2: type =&gt; post</li>
<li>Item 3: type =&gt; post</li>
<li>Item 2: category =&gt; Allgemeines</li>
<li>Item 3: category =&gt; Spezielles</li>
<li>Item 3: tag =&gt; Wichtig</li>
</ul>
<p> </p>
<p>SQL ist eine sehr mächtige Sprache. Im Prinzip kann man die gesamte Logik der so erstellten Relationen in die Datenbank auslagern. Im angeführten Query werden die Taxonomien mit den jeweilig referenzierenden Termen verbunden (mit einem Unterstrich getrennt) in ein eigenes Feld mit dem Spaltenheader "type" des Result Set geschrieben.</p>
<pre>SELECT item.*, <br />
GROUP_CONCAT( ( SELECT taxonomy FROM term_taxonomy WHERE id=tr.taxonomy_id ), '_', ( SELECT id FROM term WHERE id=tr.term_id ) ) AS type_int,<br />
GROUP_CONCAT( ( SELECT taxonomy FROM term_taxonomy WHERE id=tr.taxonomy_id ), '_', ( SELECT name FROM term WHERE id=tr.term_id ) ) AS type_str<br />
FROM item<br />
INNER JOIN term_relation tr ON tr.object_id=item.id<br />
GROUP BY item.id<br />
HAVING type LIKE ('%type_%')</pre>
<p>Mit einer HAVING Klausel kann man Aliase auf Treffer matchen da WHERE hier syntaktisch falsch wäre. Und außerdem - je mehr Arbeit die Datenbank erledigt, desto schneller und ressourcenschonender läuft das Script. PHP hat Limitierungen wie Buffer, Ausführungszeit und vieles andere, für die Datenbank ist der Query lächerlich. Ich finde ein CMS sollte wirklich nur den Content managen, deshalb nennt man es ja auch so.</p>
<p> </p>
<h3>Eigene Taxonomien erstellen</h3>
<p>Eigene Taxonomien kann man einfach über das Admin Interface anlegen. Ich hab zum Beispiel die Taxonomie tag angelegt. Im Template single.php kann ich die zugehörigen Tags dann anzeigen. Die Links zeigen automatisch auf URIs, die das entsprechende Archiv anzeigen. Also alle Inhalte, die die selbe Taxonomie -&gt; Term Relation aufweisen. Wird kein Term angegeben werden alle Inhalte angezeigt die zumindest die selbe Taxonomie haben.</p>
<pre>if( $categories = $this-&gt;terms( &#039;category&#039; ) ) {<br />echo "&lt;div class=&#039;sp-sidebar-item&#039;&gt;";<br />    echo "&lt;div class=&#039;sp-sidebar-item-head&#039;&gt;Kategorien&lt;/div&gt;";<br />    echo "&lt;div class=&#039;sp-sidebar-item-box&#039;&gt;\n";<br />    foreach( $categories as $category ) { echo "&lt;div class=&#039;sp-sidebar-item-box-head&#039;&gt;&lt;a href=&#039;../?category=$category[id]&#039;&gt;$category[name]&lt;/a&gt;&lt;/div&gt;"; }<br />    echo "&lt;/div&gt;\n";<br />echo "&lt;/div&gt;";<br />}</pre>
<p>Es kommt aber noch eine Funktion bzw. sogar eine neue Klasse um diese Taxonomie Abrufe zu automatisieren.</p>
<p>Erfindet zwar nicht das Rad neu, ist aber extrem effizient!</p>
