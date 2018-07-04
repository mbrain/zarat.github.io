---
layout: post
title: Hooks, Aktionen und Filter
author: Manuel Zarat
category: tutorials
tags: protokoll programm
permalink: /blog/hooks-aktionen-und-filter
---

<p>Durch Hooks, Aktionen und Filter kann man sich in den Core-Code einhaken und das Verhalten des Seitenaufbau beeinflussen. Hooks können verwendet werden damit der Core-Code unverändert bleibt und die Modifikationen auch nach einem Update erhalten bleiben. Möchte man den Core-Code ändern, kann man das auch und an jeder beliebigen Stelle seine eigenen Hooks setzen. Es gibt grundsätzlich zwei Arten von Hooks - Action Hooks und Filter Hooks.</p>
<!--excerpt_separator-->
<h3>Action Hooks</h3>
<p>kommen zum Einsatz, wenn zu einer bestimmten Aktion im Core-Code eine zusätzliche Akton ausgeführt werden soll. So kann man z.B eine Rundmail versenden, sobald ein neuer Inhalt erscheint o.ä.</p>
<h3> </h3>
<h3>Filter Hooks</h3>
<p>verändern Daten, die bereits bestehen. Diese veränderten Daten müssen der Ursprungsfunktion dann wieder zurückgegeben werden, die dann wie gewöhnlich fortfährt. So kann man die Metadaten eines Items durch einen Filter so manipulieren, das dem Titel immer ein bestimmtes Wort vorangestellt wird oder bei Portfolio Seiten Bildergallerien einbinden o.ä.</p>
<h3> </h3>
<h3>Eigene Hooks anlegen</h3>
<p>Um Hooks zu erstellen oder aufzurufen muss man die globale Variable $hooks initialisieren. Um einen Action Hook zu setzen, suchen Sie die Funktion die Sie modifizieren möchten, rufen Sie die Funktion do_action() auf die globale $hooks Variable auf und geben ihr einen Namen.</p>
<pre>global $hooks;
$hooks-&gt;do_action( &#039;meine_action&#039; );</pre>
<p>Im Theme können Sie dann Ihre eigene Funktion in diesem Hook enhaken. Auch dabei muss wieder die globale Variable $hooks initialisiert werden.</p>
<pre>global $hooks;
$hooks-&gt;add_action( &#039;meine_action&#039;, &#039;meine_funktion&#039; );
function meine_funktion() {
    echo "Meine Action wurde aufgerufen!";
}</pre>
<p>Filter Hooks können auch bei jeder Funktion eingefügt werden. Sie machen hald nur Sinn, wenn die Funktion Daten verarbeitet bzw. Variablen bereitstellt. Um einen Filter zu setzen benutzt man die Funktion apply_filters() Diesem Filter wird wieder ein Name und zusätzlich auch Daten mitgegeben. Die Daten können Objekte, Arrays oder einfache Strings sein, je nach dem was der Filter machen soll.</p>
<pre>global $hooks;
$hooks-&gt;apply_filters( &#039;mein_filter&#039;, $daten );</pre>
<p>Im Theme kann man den Filter entweder innerhalb einer Funktion aufrufen oder auch im globalen Namensraum. Deshalb ist die Variable $hooks auch global.</p>
<pre>global $hooks;
$hooks-&gt;add_filter( &#039;mein_filter&#039;, &#039;meine_funktion&#039; );
function meine_funktion( $daten ) {
    return "Mein Filter hat " . $daten . " aufgerufen also werden sie gefiltert.";
}</pre>
