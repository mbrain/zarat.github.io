---
layout: post
title: Einen OpenLDAP Verzeichnisdienst erstellen
author: Manuel Zarat
tags: protokoll
permalink: /blog/openldap-verzeichnisdienst-erstellen
---

<p>OpenLDAP ist ein Datenbank Backend zum Verwalten von Organisationseinheiten wie Benutzern, Rechnern u.ä in größeren Organisationen. In Verbindung mit Samba ist es ein vollwertiger Ersatz für ein Windows Active Directory.</p>

<!--excerpt_separator-->

<h1>Installation</h1>

<p>Zuerst installiere ich LDAP und die dazugehörigen Programme</p>

<pre>apt-get install slapd ldap-utils</pre>

<p>und starte zu Beginn gleich die Erstkonfiguration</p>

<pre>dpkg-reconfigure slapd</pre>

<p>Dabei werde ich nach Domainname und Passwort des Domänenadministrators gefragt.&nbsp;Um zu testen ob die Installation geklappt hat, kann man direkt die Konfiguration anzeigen</p>

<pre>ldap­se­arch -Y EXTERNAL -H ldapi:/// -b "cn=config"</pre>

<h1>Eine erste OU anlegen</h1>

<p>Um eine OU anzulegen, muss man eine ldif Datei anlegen. Ich erstelle zu Beginn eine OU für alle Benutzer und eine für Benutzergruppen. Beispiel: ou.ldif</p>

<pre>dn: ou=People,dc=example,dc=com
objectClass: organizationalUnit
ou: People

dn: ou=Groups,dc=example,dc=com
objectClass: organizationalUnit
ou: Groups</pre>

<p>Diese Datei importiert man mit ldapadd</p>

<pre>ldapadd -x -D "cn=admin,dc=example,dc=com" -w &lt;password&gt; -H ldap:// -f ou.ldif</pre>

<h1>Eine Gruppe anlegen</h1>

<p>Mit Gruppen von Objekten fällt es leichter, diese zu verwalten und einheitlich zu konfigurieren. Beispiel: group.ldif</p>

<pre>dn: cn=Employees,ou=Groups,dc=example,dc=com
objectClass: posixGroup
cn: employees
gidNumber: 5000</pre>

<h1>Benutzer hinzufügen</h1>

<p>Möchte man einen Benutzer zu einer bestehenden OU hinzufügen, erstellt man auch eine ldif Datei. Beispiel user.ldif</p>

<pre>dn: uid=zarat,ou=People,dc=example,dc=com
changetype: add
objectClass: inetOrgPerson
description: Manuel Zarat - Standard Account.
cn: Manuel Zarat
sn: Zarat
uid: zarat
gidNumber: 5000</pre>

<p>Diese Datei importiert man wieder mit Hilfe von ldapadd</p>

<pre>ldapadd -x -D "cn=admin,dc=example,dc=com" -w password -H ldap:// -f user.ldif</pre>

<h1>Benutzer verschieben</h1>

<p>Möchte man einen Benutzer verschieben, muss man wieder eine eigene ldif Datei anlegen. moveuser.ldif</p>

<pre>dn: uid=zarat,ou=People,dc=example,dc=com
changetype: modrdn
newrdn: uid=zarat
deleteoldrdn: 0
newsuperior: ou=Customers,dc=example,dc=com</pre>

<p>und wieder mit Hilfe von ldapadd importieren</p>

<pre>ldapadd -x -D "cn=admin,dc=example,dc=com" -w password -H ldap:// -f moveuser.ldif</pre>

<h1>Eine OU löschen</h1>

<p>Auch zum Löschen einer OU muss eine ldif Datei erstellt werden. Beispiel: delete_ou.ldif</p>

<pre>dn: ou=People,dc=example,dc=com
changetype: delete</pre>

<p>Importieren mit ldapadd</p>

<pre>ldapadd -x -D "cn=admin,dc=example,dc=com" -w &lt;password&gt; -H ldap:// -f delete_ou.ldif</pre>
