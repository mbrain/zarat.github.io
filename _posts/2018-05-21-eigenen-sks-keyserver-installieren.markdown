---
layout: post
title: Einen SKS Keyserver installieren
author: Manuel Zarat
category: tutorials
permalink: /blog/eigenen-sks-keyserver-installieren
published: true
---

Der SKS Keyserver lässt sich bequem über den Linux Paketmanager installieren.
<!--excerpt_separator-->

apt-get install sks

Weil der Dienst nach der Installation automatisch startet, beende ich ihn vorerst

service sks stop

Während der Installation wurde ein Benutzer debian-sks angelegt. Unter diesem installieren wir den Server mit

su debian-sks -c '/usr/sbin/sks build'

Da mein Schlüsselserver sich nicht mit anderen synchronisieren soll ersetze ich noch den Inhalt folgender Dateien

echo '# Empty - Do not communicate with other keyservers.' >/etc/sks/mailsync
echo '# Empty - Do not communicate with other keyservers.' >/etc/sks/membership

Nun noch einige kleine Parameter setzen

cat >/etc/sks/sksconf <<'EOF'
pagesize: 16
ptree_pagesize: 16
EOF

als Service registrieren

systemctl enable sks.service

und den Autostart aktivieren.

echo 'initstart=yes' >/etc/default/sks

Der Schlüsselserver ist somit einsatzbereit und kann mit

service sks start

gestartet werden und damit ist unser Schlüsselserver auch schon bereit ihn zu testen.
