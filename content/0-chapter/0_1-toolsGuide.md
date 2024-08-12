---
title: "Tools Guide"
weight: 10
pre: "0.1. "
---

## GitHub account
First, you will need to create a GitHub account [here](https://github.com/). If you already have one, you can use your existing account.

Your GitHub account will need to be set up to use two-factor authentication (a new requirement). When logged into your GitHub account (link above), click your icon in the upper-left corner (it will say "Signed in as...(your account name)"). Select "Settings" and then "Account security". Scroll down until you find "Two-factor authentication". If it is disabled, then enable it. I use an SMS number, but you are welcome to use an authenticator app instead (I'm not familiar with that process, though).


## Sireum Logika

In CIS 301, we will use a tool called Logika, which is a verifier and a proof checker for propositional, predicate, and programming logic. You will need to install the IntelliJ-based Sireum IVE (Integrated Verification Environment), which contains Logika.

### Getting Installer Script

Go [here](https://sireum.org/getting-started/#for-courses) to install Logika. 

Under "For K-State CIS 301 & 706/801, Fall 2024", click either "Windows" or "macOS/Linux". The instructions below should update based on your selection. Copy the command listed for your operating system.

### Installing and Running Sireum Logika - Windows

For Windows users, open a command prompt and switch to the C:\ drive by typing:

```
cd C:\
```

Next, paste the command you copied in the previous step. You should immediately see feedback that your computer is fetching Sireum Logika.

Once the installation is finished, open a file explorer and navigate to "C:\Sireum\bin\win\idea\bin\". In that folder, you should see the application "IVE.exe". Double-click that application to run it (if you see a popup that asks if you want to allow access to your computer, go ahead and choose that you want to do so).

You may find it handy to pin the "IVE.exe" application to your taskbar. You can do so by right-clicking the "IVE.exe" file and selecting "Pin to taskbar".
 

### Installation and Running Sireum Logika - Mac

For Mac users, open a terminal window and paste the command you copied in the previous step. You should immediately see feedback that your computer is fetching Sireum Logika.

Once the installation is finished, go to the finder and search for "IVE". You should see the application "IVE.exe" - if you run it, it will open Sireum Logika.

## Using GitHub to start homework assignments

To start a homework assignment (or to clone any existing repository, including homework solutions and lecture examples), first:
- Click to "accept the assignment" from the link in Canvas (if you are cloning a homework assignment or solution).
- Go to the URL created for your assignment (or to the URL of the repository you want to clone).
- You should be looking at a website that looks something like this:

![GitHub assignment](/images/gitHubAssign.png)

Click the Green *Code* button, so that you see something like this:

![GitHub clone](/images/gitHubClone.png)

Click the clipboard icon next to the listed URL to copy it to your clipboard.
Next, open Sireum IVE. You should see:

![Open Sireum](/images/openSireum.png)

Click *Get from VCS*. Now you should see something like:

 ![Get from VCS](/images/getFromVCS.png)

Paste the URL you copied from GitHub in the *URL* textbox above. 
Under *Directory*, navigate to a folder on your computer specifically for CIS 301 (create one if it doesn't exist). Create a new empty folder within that CIS 301 folder to hold this new project. Select that new folder in the *Directory* box, so that you now have something like this:

 ![Clone to Sireum](/images/cloneToSireum.png)

Click *Clone*. You should see a popup like this:

![Login via GitHub](/images/loginViaGitHub.png)

Click *Use Token*. Now you see:

 ![Generate token](/images/generateToken.png)

Click *Generate...* This will bring up a browser asking you to login to your GitHub account. Do so there. It will then bring up a page about a *New personal access token*. Edit the expiration date of the token so that it expires after the end of the semester. Scroll down to the bottom of that page and click *Generate token*.

This should bring a new page that says *Personal access tokens*. There should be a token that is highlighted in green with a clipboard icon next to it. Click the clipboard icon to copy the token. If you get an error in the GitHub page when creating your token that says, *Note has already been taken*, just change the listed note to anything else (it says *IntelliJ IDEA GitHub integration plugin* by default, but you can edit it to whatever you want.

Go back to the dialog in Sireum/IntelliJ and paste in your token in the textbox. Click *Log In*.  You should see something like this:

 ![Open project ](/images/openProject.png)

If you don't see the panel on the left side, click the *Project* label on the left border. Expand the *src* folder to see your starting files. Now you can double-click one of those src files to view and edit it.

## Check your GitHub settings in Sireum IVE

After successfully cloning a GitHub repository, check your GitHub settings in Sireum/IntelliJ. Go to File→Settings, then Version Control, then GitHub. If you see your GitHub account listed, like this, then you are done:

 ![Check settings](/images/checkSettings.png)

If you don't see your GitHub account, click the + icon and then *Login with token...*. Paste in the same personal access token you used before, and select *Add Account*:

 ![Add account](/images/addAccount.png)

## Committing and pushing changes

When you are finished working, commit and push your changes to GitHub. (I recommend doing this anytime you are at a stopping point, as well as when you are completely done.)

To do this, select *Git* in the menu, then *Commit*. Type in something under *Commit Message*. Click the down arrow next to the Commit button at the bottom, and select *Commit and Push*:

 ![Commit and push](/images/commitAndPush.png)

In the resulting dialog, click *Push*. The first time you commit and push in IntelliJ, it will likely ask you to enter your name and email (this is just for documentation purposes in your repository). Enter them in the dialog and say OK.

If you go back to the URL of your GitHub repository and refresh the page, you should see your latest work.

Homework 0 will help you test the GitHub process. 

## Cloning additional repositories

Sireum IVE looks a little different after it has been run the first time. If you want to clone a second (or third, etc.) repository, first open Sireum. It will automatically open your most recent project.

To clone a new project, select *File→New→Project* from *Version control*. Then, follow the instructions in *Using GitHub to start homework assignments* above. You will likely not have to do anything with a personal access token or with entering information about your GitHub account. If you do have trouble working with additional repositories, though, I recommend trying to generate a new personal access token when prompted.

## Updating Proyek settings in later repositories

When we start using the Logika tool to check our work on truth tables and proofs, you will need to take an additional step after cloning a new repository. First, wait for Sireum to say it is finished indexing your project. Next, you should see a popup that says something about Proyek reload. Click that you want to restart the IVE. When it restarts, you should be able to work as usual.

If you skip the Proyek reload step, you may notice red error markings on the lines of your proof. This won't actually affect their correctness, but it can be very distracting. If you ever notice these red error markings and don't see the Proyek reload option, you can manually reload it by choosing the menu options Tools->Sireum->Syncrhonize Proyek.

## Using Sireum IVE in the lab classrooms

Sireum IVE (with Logika) is available in the CS computer lab classrooms (DUE 1114, 1116, and 1117). To find it, open a File Explorer, then open the C:\ drive. From there, navigate to "C:\Sireum\bin\win\idea\bin\" and run the "IVE.exe" file. This will launch Sireum as usual.

If you want to complete your assignment using multiple computers, you can:
    - Clone the repo using the process above (if it is your first time working on the current assignment on this computer)
    - Commit/push your changes when you are ready to leave this computer
    - Do Git->Pull to get your latest changes if you come back to a different computer

## Remote access

If you have any problems installing Sireum on your own computer, you can access it remotely from the CS department. See [here](https://support.cs.ksu.edu/CISDocs/wiki/Remote_Access) for information on remote access. When you connect to the department Window's server, you can follow the instructions above to find Sireum as you would on a lab computer.