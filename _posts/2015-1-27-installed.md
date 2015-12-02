---
layout: post
title: glance install
date: 2015-01-27
tags: [management tool], [observation]
categories: [tools]
---

マシンのリソース使用状況がcommand line上で見られる


> -参考) [command line - System Monitoring Tools For Ubuntu - Ask Ubuntu](http://askubuntu.com/questions/293426/system-monitoring-tools-for-ubuntu)-


### - How to install and use
	% sudo apt-get install python-pip build-essential python-dev
	% sudo pip install Glances
	% sudo pip install PySensors
	% glances

	In glances you’ll see a lot of information about the resources of
	your system: CPU, Load, Memory, Swap Network, Disk I/O and
	Processes all in one page, by default the color code means:

	GREEN : the statistic is “OK”
	BLUE : the statistic is “CAREFUL” (to watch)
	VIOLET : the statistic is “WARNING” (alert)
	RED : the statistic is “CRITICAL” (critical)

	When Glances is running, you can press some special keys to give
	commands to it:

	c: Sort processes by CPU%
	m: Sort processes by MEM%
	p: Sort processes by name
	i: Sort processes by IO Rate
	d: Show/hide disk I/O stats
	f: Show/hide file system stats
	n: Show/hide network stats
	s: Show/hide sensors stats
	b: Bit/s or Byte/s for network IO
	w: Delete warning logs
	x: Delete warning and critical logs
	1: Global CPU or Per Core stats
	h: Show/hide this help message
	q: Quit (Esc and Ctrl-C also work)
	l: Show/hide log messages
