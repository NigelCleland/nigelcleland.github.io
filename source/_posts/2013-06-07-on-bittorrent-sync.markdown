---
layout: post
title: "On Bittorrent Sync"
date: 2013-06-07 12:21
comments: true
categories: [Bittorrent sync]
---

So Bittorrent Sync is the newest offering from BitTorrent and I must say I am in love. Broadly, it allows you to synchronise folders between multiple computers quickly and easily. In this way it is very similar to Dropbox and other equivalents (Drive, SkyDrive, Box, iCloud) etc. Now those other services have their own lists of benefits and negatives which I'm not going to cover here. Instead, this post is about BitTorrent Sync.

First, Bittorrent has been given a bit of a bad rep in recent years due to its proliferation in the use of internet piracy. However, such a use is technology agnostic and a number of other individuals and companies are using BitTorrent for entirely legitimate purposes. Simply, it reduces the requirement for centralised servers for downloading and sharing files between multiple people. In effect, cutting down on costs and bandwidth requirements for people who have many large files to share.

<!-- more -->

Notable users include many linux distributions, World of Warcraft and the BBC iPlayer (these are all from memory, they may have changed). Simply by redistributing the burden of distributing files to the community multiplier network effects can be induced. For example, the more people who are sharing a particular download the fast that distribution can be expected to occur. As many people do not fully utilise their bandwidth and connections such sharing is a simple way of piggybacking off the existing infrastructure.

Other benefits include the potential to use local versions (instead of grabbing a copy off the US and using limited international bandwidth) the abundant local fibre networks can be used. This is similar in principle to an ISP maintain a local Content Delivery Network for large downloaded files (for example streaming services/steam/etc). So what on earth do these two things have in common?

Enter BitTorrent Sync.

BitTorrent Sync incorporates the power of the bittorrent protocol along with the ease of use of Dropbox and its ilk. Furthermore, as everything is local it has the following advantages.

1. You can sync everything over a LAN if available (e.g. don't need to use limited bandwidth)
2. No size limits!
3. Can share with as many, or as few, people as desired making it perfect for collaborating on documents etc where a full version control system such as git would be overkill.
4. All files are stored locally so you can safely share sensitive information without use of a central server.
5. Control
6. One way synchronisation

## Syncing over LAN.

This one here is crucial for me. In New Zealand we have very limited, very expensive bandwidth caps which make the constant syncing of large changes to DropBox folders a major pain in the proverbial. Being able to Sync everything over LAN means I can set up my backups incredibly simply (I use a RPi powered set up for this) and generally just take the hassle out of everything.

## No Size Limits! 

I have GBs of raw data which I'm continuously doing analysis on, sometimes on different computers and platforms. Simply having a consistent space for all of my data so that I can be assured that what works on one computer will work on another is an absolute pleasure. 

## Unlimited, personalised sharing

I know I know DropBox etc can do this to. However it's still a major plus that I can set up a customised sharing lists both with myself and with other people (e.g. different folder sharing to share photos with friends and loved ones). I'm not personally a fan of the "put everything on Facebook" style of sharing and prefer to share high quality original files where appropriate. This makes it easy, and as an added bonus I keep control of my files and documents.

## Local Versions only 

All BitTorrent sync traffic is fully encrypted. Thus, you can feel safe that all files shared are only going to be available to those people you want to see them. No worries about leaving things on usb drives, or sending them via email attachments. Simply just share the folder or file directly with bittorrent sync. 

## Control 

I know a lot of people don't care about this, however from a personal point of view I adhere to the "If the product is free, then you are the product" mantra of the internet. Being solicited for advertisements based upon the files I'm sharing for work/personal purposes is not something I ever want to happen in the future. Nor do I have to worry about what happens if a Service such as DropBox or Google Drive change their terms of service. As such I'm much less locked into the Sync platform than the others, this low friction environment is preferential for personal reasons to me.

## One Way Synchronisation

There are many occasions when I want someone to be able to see what I'm working on, but don't want them to modify my source. If two way synchronisation is enabled by default then I have to maintain my own local version to ensure that someone doesn't accidentally wipe my shared version. This is a major PITA and a source of friction and can often lead to the dreaded filename_author_date_version_latest_seriously_SERIUOSLY.ext filenaming system which I have seen utilised quite often. Quite simply this just isn't an acceptable way of conducting business. At all.

## Convinced yet?

Convinced? Give it a try, it's free! You can find the download and other material at the [BitTorrent Sync](http://labs.bittorrent.com/index.html) website.


