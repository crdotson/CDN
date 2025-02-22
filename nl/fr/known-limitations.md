---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limitations connues
{: #known-limitations}

Les limitations suivantes s'appliquent au service CDN avec le fournisseur Akamai :
* La purge de contenu au niveau des répertoires ou de plusieurs fichiers n'est actuellement pas prise en charge.
* Le nombre total d'origines par CDN est limité à 75.
* HTTPS avec un certificat SAN DV n'est disponible que pour les nouveaux CDN.
* La protection des liens dynamiques ne prend pas en charge les termes de correspondance d'URL `refererValues` dans lesquels le caractère défini devant le premier caractère `.` contient le regroupement de caractères `://`, par exemple `http://www.example.com` ou `www.example.com http://www.example.com`
