---
title: Powershell script to send emails
author: michael
layout: post
date: 2013-01-24
url: /2013/01/powershell-script-to-send-emails/
categories:
  - Code
tags:
  - Powershell

---
Since Microsoft in their infinite wisdom have decided to deprecate the &#8220;Send Email Message&#8221; function from their Event Viewer&#8217;s attached tasks with Windows 8 / Server 2012, all that&#8217;s left is to run an application or a script instead. While this annoyed me greatly in the beginning, once I started playing with Powershell it turned out to be not so bad. As a matter of fact, it turned out to be rather nice. Powershell really is quite powerful.

So here&#8217;s a very simple script that&#8217;ll read all relevant parameters and send your message (e.g. the event) via email. Figuring out how to use the credentials was a bit tricky (gmail needs those), but in the end, rather logical.

``` powershell
    Param(
        [string]$subject         =  "PowerShell Notification from Server",
        [string]$to              =  "el_capitan@x.com",
        [string]$from            =  "lowly_servant@x.com",
        [string]$smtpserver      =  "smtp.gmail.com",
        [string]$body            =  "There was a message here somewhere, I'm sure about it...",
        [int32]$port             =   587,
        [string]$username        =  "lowly_servant@x.com",
        [string]$password        =  "secretdeluxe"
    )

    $secpasswd = ConvertTo-SecureString $password -AsPlainText -Force
    $mycreds = New-Object System.Management.Automation.PSCredential ($username, $secpasswd)

    Send-MailMessage -From $from -Subject $subject -To $to -SmtpServer $smtpserver -UseSsl -Body $body -Port $port -Credential $mycreds
```