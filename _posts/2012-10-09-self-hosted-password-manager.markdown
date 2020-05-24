---
layout: post
title: "Self-hosted password manager"
author: madan
categories: [ Software]
description: This was a weekend project to build a password manager that can be self hosted on google app engine.
image: assets/images/posts/2012-10-09-self-hosted-password-manager/personal-password-manager.png
---

The most reused password used in the world is 123456. There are only a few different options when it comes to choosing your password and remembering them. We will walk through a few options for creating and storing your password. Finally, I will introduce a web based password manager that I created over a weekend as a fun project. It can be self hosted for free while providing the best parts of all the available options to store and secure your passwords.

<!--excerpt_separator-->

## Create passwords and memorize

Humans reuse the same password and sometimes relatively simple ones. Those who are sensitive about security innovate on their password patterns. If you have a good memory, this is an option, not the best though. Attackers can extract patterns if they have access to a few of your passwords.

## Use single identity providers

Single identity provider systems like [openId][8] should have been the norm as it provides a great way to keep your identity secure while getting access to large number of website. The adoption of this technology did not unfold the way it was envisioned. The failure can be attributed to two reasons. The need for a single company providing the identity, e.g., openId, facebookConnect. The other reason is that smaller websites would be least bothered to spend the effort to integrate with these systems which are fairly more involved that just storing salted encrypted passwords in the database.


## Master password with hosted service

There are a number of companies that provide hosted and self-hosted options to secure all your passwords behind a master password. This is by far the most widely adopted option for the average security conscious person. Companies like lastpass, 1password provide this option. The only issue is that it provides the attacker a single large pot of gold. With chrome extensions provided by these companies, it could result in [security vulnerabilities][9]. It is well known that your data is as secure as your weakest link.

Those who are uncomfortable with hosted solutions, can opt for self-hosted solutions. For individuals it can be expensive for no clear ROI.

One of the free option to store your own password vault is using keepassX[2]. It is a great software that has good support across all platforms, both mobile and desktop. The main issue of this system is synchronization of passwords file. I have had situations where the file got corrupted while synchronizing it using dropbox.

## Best of all options

None of the above options seem perfect. The ideal system should have the best features of all those systems. It should be free, should not be the single stop pot of gold for hackers and should not lock us into a single provider (like openID).

This led me to create my own [password manager][1] that can be self hosted on any free providers like heroku or google app engine. This provides the necessary flexibility of having a web based service that is secure and free, that almost no one but you would know even exists.

The implementation has a UI that encrypts the password using your master password and [Stanford Javascript Crypto Library][7]. The backend is a python [Flask][4] application that receives an already encrypted password via https and just stores it in the backend in a blob store. Here is the UI of the application.

![self hosted password manager](/assets/images/posts/2012-10-09-self-hosted-password-manager/password-manager.png){:width="336px"}{: .center-image }

I did not spend time optimizing the specific crypto technique as I was more focused on getting the application to work end to end over the weekend. I am sure there will be opportunities to improve on the specific crypto options.

In order to lookup/store passwords:
1. you must be logged in into your google account.
2. you must also know your mater password.

I have been fairly happy with the functionality is has been providing me with the layers of security. You can optionally configure it such that you would not need your google account to be logged in.

## Guide to have one of your own hosted password manager
- [Github Source][1]
- [Slideshare – Installation guide][5]
- [Slideshare – How to guide][6]

[1]: https://github.com/HackingHabits/PersonalPasswordManager
[2]: http://www.keepassx.org/
[3]: https://lastpass.com/
[4]: http://flask.pocoo.org/
[5]: http://www.slideshare.net/hackinghabits/personal-passwordmanager-installation-14646084
[6]: http://www.slideshare.net/hackinghabits/password-manager-howtoguide
[7]: https://bitwiseshiftleft.github.io/sjcl/
[8]: http://openid.net/what-is-openid/
[9]: https://blog.lastpass.com/2017/03/security-update-for-the-lastpass-extension.html/
