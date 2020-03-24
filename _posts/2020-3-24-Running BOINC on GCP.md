---
title: Running BOINC on free GCP instance
categories: Cloud
published: true
---
The calculation of proteins, better known as Folding@Home, has recently become very popular again. The trigger was certainly the corona crisis.
Some colleagues at VMware have developed a Fling for this purpose, which is also becoming increasingly popular.

But not everyone has a Homelab with sufficient computing power available. Therefore I have been thinking about moving this workload into the cloud for quite some time. 

After a hint of a colleague to the entry offer of the Google Cloud Platform, I have prepared the "always free f1-micro" instance there for BOINC. Sure, a virtual CPU is not much, but sometimes it's the stamina and not the sprint that counts.

Here I want to show you how everyone can use the instance and control it from home.

First you have to create a Google Cloud Account, which is really not difficult and is explained e.g. in [this](https://medium.com/@hbmy289/how-to-set-up-a-free-micro-vps-on-google-cloud-platform-bddee893ac09) blog entry.

