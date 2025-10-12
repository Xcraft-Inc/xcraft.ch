---
title: 'Avant-propos'
weight: 10
tags: ['carnotzet']
pre: '<b>5.1 </b>'
---

Le **Carnotzet OS** est un système d'exploitation simple et sans prétention,
entièrement construit à l'aide du toolchain Xcraft et basé sur GNU/Linux. Il
offre une expérience complète pour une utilisation sur (principalement) des
systèmes embarqués modernes. Son objectif n'est pas de remplacer des
distributions telles que Raspbian, mais de mettre à disposition une distribution
entièrement compilable depuis les sources, minimaliste (dans le sens positif du
terme) et configurable.

## Composants

Les briques de **Carnotzet OS** sont très communes. On y trouve [U-Boot][uboot]
pour le chargement du noyau (bootloader), on y ajoute ce bon vieux noyau
[Linux][linux] et on l'assaisonne avec [BusyBox][bb]. La libc utilisée est la
[glibc][glibc]. Un gestionnaire de paquets très inhabituel est utilisé ici et se
nomme [wpkg][wpkg]. Il n'y a rien d'extraordinaire mais j'ose espérer que les
chapitres suivants vous permettront d'apprécier ce système d'exploitation. Que
l'on se comprenne bien, **Carnotzet OS** n'est pas un jouet. Ce système est prêt
à être utilisé en production.

## Buts

Ce système est encore très jeune. Il est disponible uniquement pour Raspberry Pi
4B. Il est prévu de l'adapter pour d'autres systèmes embarqués et tout ceci
viendra en temps voulu. Des rumeurs parlent du support pour le Raspberry Pi 5,
Raspberry Pi Zero ainsi que pour les cartes NVIDIA Jetson Orin Nano.

Son but principal est d'y faire fonctionner quelques services très spécifiques.
Par exemple, **Carnotzet OS** pourrait être utilisé pour faire fonctionner des
services web écrit en Node.js. Les vecteurs d'attaques sont très faibles car le
système n'expose aucun listeners réseau avec son installation de base.

## Mais encore

Veuillez consulter la section [Installation](/carnotzet/02.installation) si vous
avez envie de tenter le coup.

[uboot]: https://u-boot.org/
[linux]: https://kernel.org
[bb]: https://www.busybox.net/
[wpkg]: https://github.com/Xcraft-Inc/wpkg
[glibc]: https://www.gnu.org/software/libc/
