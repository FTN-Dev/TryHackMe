# Sakura Room Write Up

## Question 1 - BACKGROUND:

### Background

This room is designed to test a wide variety of different OSINT techniques. With a bit of research, most beginner OSINT practitioners should be able to complete these challenges. This room will take you through a sample OSINT investigation in which you will be asked to identify a number of identifiers and other pieces of information in order to help catch a cybercriminal. Each section will include some pretext to help guide you in the right direction, as well as one or more questions that need to be answered in order to continue on with the investigation. Although all of the flags are staged, this room was created using working knowledge from having led and assisted in OSINT investigations both in the public and private sector. 

NOTE: All answers can be obtained via passive OSINT techniques, DO NOT attempt any active techniques such as reaching out to account owners, password resets, etc to solve these challenges.

If you have any other questions, comments, or suggestions, please reach out to us at @OSINTDojo on Twitter.

### Instructions

Ready to get started? Type in "Let's Go!" in the answer box below to continue.

### Flag

__Let's Go!__

## Question 2 - Tip Off

### Background

The OSINT Dojo recently found themselves the victim of a cyber attack. It seems that there is no major damage, and there does not appear to be any other significant indicators of compromise on any of our systems. However during forensic analysis our admins found an image left behind by the cybercriminals. Perhaps it contains some clues that could allow us to determine who the attackers were?

We've copied the image left by the attacker, you can view it in your browser <a href='https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg'></a>.

### Instructions

Images can contain a treasure trove of information, both on the surface as well as embedded within the file itself. You might find information such as when a photo was created, what software was used, author and copyright information, as well as other metadata significant to an investigation. In order to answer the following question, you will need to thoroughly analyze the image found by the OSINT Dojo administrators in order to obtain basic information on the attacker.

What username does the attacker go by?

### Flag

First, lets download this image so we can analyze it. We can use the command line tool wget to do this:

```bash
wget https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg
```

Now that we have the image, we can use a tool called __exiftool__ to analyze the metadata of the image. If you don't have exiftool installed, you can find installation instructions at https://exiftool.org/.

Once you have exiftool installed, run the following command to analyze the image:

```bash
$ exiftool sakurapwnedletter.svg
ExifTool Version Number         : 12.40
File Name                       : sakurapwnedletter.svg
Directory                       : .
File Size                       : 830 KiB
File Modification Date/Time     : 2025:10:20 05:33:02+07:00
File Access Date/Time           : 2025:10:20 05:33:02+07:00
File Inode Change Date/Time     : 2025:10:20 05:33:02+07:00
File Permissions                : -rwxrwxrwx
File Type                       : SVG
File Type Extension             : svg
MIME Type                       : image/svg+xml
Xmlns                           : http://www.w3.org/2000/svg
Image Width                     : 116.29175mm
Image Height                    : 174.61578mm
View Box                        : 0 0 116.29175 174.61578
SVG Version                     : 1.1
ID                              : svg8
Version                         : 0.92.5 (2060ec1f9f, 2020-04-08)
Docname                         : pwnedletter.svg
Export-filename                 : /home/SakuraSnowAngelAiko/Desktop/pwnedletter.png
Export-xdpi                     : 96
Export-ydpi                     : 96
Metadata ID                     : metadata5
Work Format                     : image/svg+xml
Work Type                       : http://purl.org/dc/dcmitype/StillImage
Work Title                      :
```

From the output of exiftool, we can see that the attacker goes by the username __SakuraSnowAngelAiko__. 

__Flag:__ SakuraSnowAngelAiko

## Question 3 - Reconnaissance

### Background

It appears that our attacker made a fatal mistake in their operational security. They seem to have reused their username across other social media platforms as well. This should make it far easier for us to gather additional information on them by locating their other social media accounts. 

### Instructions

Most digital platforms have some sort of username field. Many people become attached to their usernames, and may therefore use it across a number of platforms, making it easy to find other accounts owned by the same person when the username is unique enough. This can be especially helpful on platforms such as on job hunting sites where a user is more likely to provide real information about themselves, such as their full name or location information.

A quick search on a reputable search engine can help find matching usernames on other platforms, and there are also a large number of specialty tools that exist for that very same purpose. Keep in mind, that sometimes a platform will not show up in either the search engine results or in the specialized username searches due to false negatives. In some cases you need to manually check the site yourself to be 100% positive if the account exists or not. In order to answer the following questions, use the attacker's username found in Task 2 to expand the OSINT investigation onto other platforms in order to gather additional identifying information on the attacker. Be wary of any false positives!

 1. What is the full email address used by the attacker?
 2. What is the attacker's full real name?

### Flag

#### First Flag
First, i put the attacker's username "SakuraSnowAngelAiko" into a web search engine <a href='https://whatsmyname.me/'>whatsmyname.me</a> to see if there were any other platforms that the attacker used this username on.From the results, i found <a href='https://github.com/sakurasnowangelaiko'>github</a> of the attacker. Lets see whats inside. I found repository that `PGP` and we can see `publickey` file. This is what inside the file

```
-----BEGIN PGP PUBLIC KEY BLOCK-----

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
23pRnPKc+41cwL3oq8yb/Fhj+biohgIp
=grbk
-----END PGP PUBLIC KEY BLOCK-----
```

i save it to the file `gpg.asc` and import it to `gpg` by using this command:

```bash
❯  gpg --import gpg.asc
gpg: directory '/home/ruscy/.gnupg' created
gpg: keybox '/home/ruscy/.gnupg/pubring.kbx' created
gpg: /home/ruscy/.gnupg/trustdb.gpg: trustdb created
gpg: key ECDD0FD294110450: public key "SakuraSnowAngel83@protonmail.com" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

then we can see the email address of the attacker is __SakuraSnowAngel83@protonmail.com__. This is the first flag.

#### Second Flag

To find the real name of the attacker, I try to search on search engine with the full name. I found the attacker's profile on <a href='https://www.facebook.com/search/top/?q=Aiko%20Abe'>Facebook</a>. The attacker's full real name is __Aiko Abe__. This is the second flag.

## Question 4 - Unveil

### Background

It seems the cybercriminal is aware that we are on to them. As we were investigating into their Github account we observed indicators that the account owner had already begun editing and deleting information in order to throw us off their trail. It is likely that they were removing this information because it contained some sort of data that would add to our investigation. Perhaps there is a way to retrieve the original information that they provided? 

### Instructions

On some platforms, the edited or removed content may be unrecoverable unless the page was cached or archived on another platform. However, other platforms may possess built-in functionality to view the history of edits, deletions, or insertions. When available this audit history allows investigators to locate information that was once included, possibly by mistake or oversight, and then removed by the user. Such content is often quite valuable in the course of an investigation. In order to answer the below questions, you will need to perform a deeper dive into the attacker's Github account for any additional information that may have been altered or removed. You will then utilize this information to trace some of the attacker's cryptocurrency transactions.

 1. What cryptocurrency does the attacker own a cryptocurrency wallet for?
 2. What is the attacker's cryptocurrency wallet address?
 3. What mining pool did the attacker receive payments from on January 23, 2021 UTC?
 4. What other cryptocurrency did the attacker exchange with using their cryptocurrency wallet?

### Flag

#### First Flag

To find the cryptocurrency wallet information, I went to the attacker's Github profile at the Repository and i found a repository named <a href='https://github.com/sakurasnowangelaiko/ETH'>ETH</a>. So the first flag is __Ethereum__.

#### Second Flag

Inside the repository, I found a file named `miningscript` that contains the attacker's cryptocurrency wallet address. But we need to see the history, so i click on the `History` button to see the commit history of the file. From the history, I found that the attacker had changed their wallet address in a first commit. By clicking on the first commit, I was able to see the old wallet address. The attacker's cryptocurrency wallet address is __0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef__. This is the second flag.

#### Third Flag

To find the mining pool that the attacker received payments from on January 23, 2021 UTC, Am trying to search this `stratum://0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef.Aiko:pswd@eu1.ethermine.org:4444` form github repository in google search, and i found this
<a href='https://etherscan.io/txs?a=0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef'>Etherscan</a> link that shows the transaction history of the attacker's wallet address. By looking at the transactions on January 23, 2021 UTC, I found that the attacker received payments from the mining pool __Ethermine__. This is the third flag. 

#### Fourth Flag

To find the other cryptocurrency that the attacker exchanged with using their cryptocurrency wallet, I looked further into the transaction history on Etherscan. I found that the attacker had exchanged Ethereum for __Tether__ (USDT) using their cryptocurrency wallet. This is the fourth flag.

__Flags:__
1. Ethereum
2. 0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef
3. Ethermine
4. Tether

## Question 5 - TAUNT

### Background
Just as we thought, the cybercriminal is fully aware that we are gathering information about them after their attack. They were even so brazen as to message the OSINT Dojo on Twitter and taunt us for our efforts. The Twitter account which they used appears to use a different username than what we were previously tracking, maybe there is some additional information we can locate to get an idea of where they are heading to next?

We've taken a screenshot of the message sent to us by the attacker, you can view it in your browser <a href='https://raw.githubusercontent.com/OsintDojo/public/main/taunt.png'>here</a>.

### Instructions
Although many users share their username across different platforms, it isn't uncommon for users to also have alternative accounts that they keep entirely separate, such as for investigations, trolling, or just as a way to separate their personal and public lives. These alternative accounts might contain information not seen in their other accounts, and should also be investigated thoroughly. In order to answer the following questions, you will need to view the screenshot of the message sent by the attacker to the OSINT Dojo on Twitter and use it to locate additional information on the attacker's Twitter account. You will then need to follow the leads from the Twitter account to the Dark Web and other platforms in order to discover additional information.

 1. What is the attacker's current Twitter handle?
 2. What is the BSSID for the attacker's Home WiFi?

#### First Flag

To find the attacker's current Twitter handle, I examined the screenshot of the message sent to the OSINT Dojo. The screenshot shows a Twitter profile with the name `AikoAbe3`. By searching for this name on Twitter, I found the attacker's current Twitter handle is __@SakuraLoverAiko__. This is the first flag.

#### Second Flag

So at the twitter of `AikoAbe3` we can see if there is  old and new screenshot post of `MD5` hashing. And in other post, we can see if AikoAbe is mentioning about `DeepPaste`, its refer to searching to the `DeepWeb`. But am not gonna do that, so we get help from the reddit that shows this <a href='https://raw.githubusercontent.com/OsintDojo/public/main/deeppaste.png'>image</a>. We can look at the Home WiFi. Lets try to find out the BSSID of this WiFi. And yea, we found this `84:AF:EC:34:FC:F8` on `Net ID`

__Flags:__
1. @SakuraLoverAiko
2. 84:AF:EC:34:FC:F8

## Question 6 - HOMEBOUND

### Background

Based on their tweets, it appears our cybercriminal is indeed heading home as they claimed. Their Twitter account seems to have plenty of photos which should allow us to piece together their route back home. If we follow the trail of breadcrumbs they left behind, we should be able to track their movements from one location to the next back all the way to their final destination. Once we can identify their final stops, we can identify which law enforcement organization we should forward our findings to.

### Instructions

In OSINT, there is oftentimes no "smoking gun" that points to a clear and definitive answer. Instead, an OSINT analyst must learn to synthesize multiple pieces of intelligence in order to make a conclusion of what is likely, unlikely, or possible. By leveraging all available data, an analyst can make more informed decisions and perhaps even minimize the size of data gaps. In order to answer the following questions, use the information collected from the attacker's Twitter account, as well as information obtained from previous parts of the investigation to track the attacker back to the place they call home.

1. What airport is closest to the location the attacker shared a photo from prior to getting on their flight?
2. What airport did the attacker have their last layover in?
3. What lake can be seen in the map shared by the attacker as they were on their final flight home?
4. What city does the attacker likely consider "home"?

#### First Flag

if u look at the first post of `@AikoAbe3` its picture of pink leaves tree. If u try to find the place, its similar `Long Bridge Park in Arlington, Virginia, United States`. Then i found out if its close to `Airport National Ronald Reagan Washington`, and the code of this airport is __DCA__.

#### Second Flag

and next, we can look at the airport pictures on `@AikoAbe3` account. If u can find or compare that pict to other pict, its from `HANEDA Airport` and the code is __HND__.

#### Third Flag

Lets open the <a href='maps.google.com'>maps</a> and compare the pict of the maps in AikoAbe's account in twitter, and we can see there is one lake that named __Lake Inawashiro__.

#### Fourth Flag
We can see at the `DeepPaste` Screenshot from the reddit before, there is City WiFi named __HIROSAKI__, and yea, this is the flag.

__Flags:__
1. DCA
2. HND
3. Lake Inawashiro
4. Hirosaki