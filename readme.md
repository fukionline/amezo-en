`Files`

`amezo.html------------------------------ Promotion of the Amezou script.`


`The following are the files required for the script.`


`amezo.cgi------------------------------- Main CGI.`

`dai.cgi--------------------------------- CGI for headline generation.`

`blist.txt------------------------------- Information files for each board.`

`header.html----------------------------- HTML for generating header parts of bulletin boards.`

# 1.
## What you need (to do / have) 
### To have
* Amezo.cgi and the associated scripts ✓
* A server that can host Perl and FastCGI scripts (and FTP to edit it)
* jcode.pl ✓
### To do
Before doing anything, let's look at the files themselves:
#### amezo.cgi
The home of the Amezou bulletin board. While it is easy to understand because it is simple and wasteless, it can also be difficult to modify in some cases.

This is what it looked like on the original website:

<img width="639" height="1180" alt="image" src="https://github.com/user-attachments/assets/ddb04291-38ea-4302-99c4-17cfb3c02d31" />

These are the functions of the bulletin board that are delegated to it:

* Create a new post thread 
* Post a response to a thread
* Generate index.html
* Reload function
*Display
#### dai.cgi
This is a script that displays the headlines for each board. For each board, it shows the top 20 threads with the most recent replies.
The board names and thread titles displayed are linked, so you can click to open them.
This CGI script also includes some impressive techniques that might make you think, 'Wow, that's clever!' — so it could be interesting to analyze it.
By the way, even if you don’t install this script, the bulletin board itself will still function.

<img width="1406" height="156" alt="image" src="https://github.com/user-attachments/assets/dc0f094c-96e0-43a8-923a-7224b54879ed" />

#### blist.txt
The file where the information for each board is set. To set a geniune example, we should inspect the file itself.

Let us take a look at the file itself.

<img width="1022" height="736" alt="image" src="https://github.com/user-attachments/assets/0ab5b8fb-1de0-4459-aaa9-0e422ab7222c" />

Someone with a sharp eye may notice that many of these correlate with the boards that *were* on the Amezo website. They would be right; we shall dissect it keeping this in mind.

<img width="428" height="631" alt="image" src="https://github.com/user-attachments/assets/ce81d617-8d1e-4347-9c0e-a1227f39ec7d" />

Now, when we look at most of the files here, we can dissect a particular pattern:

<img width="273" height="515" alt="image" src="https://github.com/user-attachments/assets/c8534b19-dd0b-4759-85fa-3e194c80a4ee" />

In question, we can tell that it must be:

**[Board directory], [Board color], [True board title]** (Board color being chosen by CSS: notice how it alternates between CSS colors and hex codes.)

And we would find ourselves completely correct using the proper references:

<img width="972" height="538" alt="image" src="https://github.com/user-attachments/assets/5f286380-40dc-4804-9dd3-c7af532c7cb2" />

But wait!!! There are some pecuilar assets hidden in here...

<img width="1022" height="212" alt="image" src="https://github.com/user-attachments/assets/bb9aa2ee-abe2-4111-9f81-4b6058a03ff8" />

The most pecuilar must be:

`.world2,#004040,<center><img src="bat2.gif" alt="わーるど２" style="position:absolute;width:150px;height:150px" id="flying"></center><object id="pathPoly4" classid="CLSID:D7A7D7C3-D47F-11D0-89D3-00A0C90833E6" style="position:absolute;visible:hidden;top:0;left:0"><param name="Target" value="flying""><param name="AutoStart" value="1"><param name="Repeat" value="-1"><param name="Bounce" VALUE="1"><param name="Duration" value="20"><param name="Shape" value="Oval(0#c#0#c#400#c#800)"></object>`

So, let's dissect that one.

The HTML is absurd, but it seems to simply be an indicator that there are no limits to how a title can look.

But there is something else there; why does it say `.world2`?

If you enter a period before your folder, it allows it to be hidden from the header.

If we look at the thing in question, this becomes clearer:

<img width="1409" height="612" alt="image" src="https://github.com/user-attachments/assets/2e2f5d99-b2cd-42c8-8955-b30bd9a8e3e7" />

Notice how there is no "Test" in the header.

Unfortunately, I couldn't find an archive that mentions `bat2.gif`. What I have said before must remain in the area of speculation.

Now, let's cover the other pecuilar ones: 

 `mousou,#000000!back027.gif,妄想`

Why is there an exclamation mark and a link following the color this time?

Well, this is simpler; let's check the website itself...

<img width="1034" height="137" alt="image" src="https://github.com/user-attachments/assets/90ba0186-2f56-4af3-94e4-1f80eb6cd6ef" />

So this will just set a custom image for the tiling image not unlike 2channel. If you put an image in the directory and set that to it, it will turn the background into a tiling version of your image.

#### header.html

This is the HTML used to generate the header section of the bulletin board. By freely customizing the contents of this HTML, you could most likely create a bulletin board with a unique, original design?
In Amezou-style boards, there's a sort of standard format for how board links are written. I’ll explain that later as well, but this header.html also serves as the base for each board's index.html. 
# 2.
Now it's time to get to the point.
From here, you will actually change the script and prepare it.
What is written here is necessary when installing it on any dirt, so please be sure to read it.
First of all, I will write only the minimum explanation that is neccessary for it to function for the current moment; coupled with what is neccessary to make it your own.
Oh, by the way, any editor you use to change is OK.
Notepad is fine, so open the following script to make changes.

### Changes in amezo.cgi

**(1)** First of all, the first line. This is a path description of the location where Perl is installed.

* **!/usr/bin/perl**

It is usually OK to leave it as it is, but if you know this will not lead to the Perl binary, you need to change it to the proper location.

Relatively many are:

* **#!/usr/bin/perl** 

* **#!/usr/local/bin/perl**

However, it is up to you to find out where it goes, if not those 2. If you say, "I cannot know which one!", I think you will find out because there is usually an explanation when you look at the site where you bought the server.

**(2)**Now, line 12.
* **$amezo = '@あめぞう (tentative)';**
This is relatively simple. It is asking you to enter the bulletin board name. One thing I should note is that this is similar to 2channel, the name of the boards bunch together. So, if you have a website called **Nippon** and a board called **Showa**, it would result in **NipponShowa**. If it is instead called **@Nippon**, however, it would be **Nippon@Showa**. Perhaps you can put **' Nippon** to result in **Nippon Showa**, but that would be absurd to look at as well, wouldn't it?

**(3)** Next, check line 13. This is the URL of the bulletin board.

* **$urlbase = '../';**

It already should link to your bulletin board since it is a self-reference; neithertheless, if you want to specify your URL, the opprtunity is and should be there just in case.

For example, if the URL of the bulletin board is **'http://hogehoge.jp/bbs/'**,

then $urlbase = **'http://hogehoge.jp/bbs/'**; it will be described.

If installed on isweb, $urlbase = **'http://hogehoge.infoseek.co.jp/cgi-bin/bbs/'**; Is it like that? These are not very relevant examples anymore, but the gist is there.

**(4)** Lastly, line 14.

* **$admin = 'Support';**

Mostly irrelevant. It is just the name you would like to be known by as the sole admin.

Let's go to line 74.

* **crypt($name, 'am') eq 'amsqKfBaSfYYU' && &del;**

This is the password you use to present your authority to the software. You must first encrypt it before you input the password! 

You must input this into a command line with Perl installed:

* **perl -le 'print crypt("secret123", "am")'**

with **secret123** becoming what you desire your password to be (do not use secret123 for lord's sake.) What is output (e.g. **amEiCwWfsRuV6**) you need to replace the string **amsqKfBaSfYYU** with. Then, preserve the unencrypted version of the password, **secret123**, and you will be able to use it when required to present authority. It does not go in reverse, so make sure to preserve the unencrypted password!

That wraps the changes needed for amezo.cgi. Let's look at another file.

###  header.html
Depending on the account being set up, in most cases, there's no need to modify header.html.
However, if the path where the bulletin board's board is set up is different from the path where the amezo.cgi script is placed, you will need to make the following changes:

**(1)** Line 292 – This line specifies the path to the amezo.cgi script.

Please modify the part that says `../amezo.cgi` in the line:

`<form method="post" action="../amezo.cgi">`

Make copies of header.html and create two files: index.html and index2.html.
You **do not** need to change the contents of these files.

### That is it!

**That...? This much?!**

Yes, if you are in a state of "just get it going for now!", this is all you need to change.

Very simple, right?

Now, finally, create the appropiate folders; we'd have hoped you already did this.

* amezo.cgi (**705**)
* header.html (**604**)
* blist.txt (**604**)
* main/ (**707**) folder
* index.html (**707**) header.html

Repeat as many bulletin boards as you want.

<img width="1421" height="546" alt="image" src="https://github.com/user-attachments/assets/ba2a4af2-a0c7-4777-8143-4ce1096c06a9" />

Did you see a screen like this?

If it doesn't come out, please review the index.html because the bulletin board URL is wrong.

Now, if you see the screen, click only the **Post** button at the bottom.


The background color and the board title will change as you had desired.

You can now use this bulletin board as much as you want.

You are the king of the peasants.

Please enjoy the thread as you like.


If you didn't rewrite it well, please review the flow so far and try again.

If you follow the previous process in order, I think it will probably go well.

Please don't give up and do your best.


Then, have a good "Amezo Life"!

