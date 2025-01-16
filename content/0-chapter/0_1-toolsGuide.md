---
title: "Tools Guide"
weight: 10
pre: "0.1. "
---

## GitHub account
First, you will need to create a GitHub account [here](https://github.com/). If you already have one, you can use your existing account.

## Sireum Logika

In CIS 301, we will use a tool called Logika, which is a verifier and a proof checker for propositional, predicate, and programming logic. You will need to install the VS Code-based Sireum IVE (Integrated Verification Environment), which contains Logika.

### Getting Installer Script

Go [here](https://sireum.org/getting-started/#cis301s25) to install Logika. 

Select either "Windows" or "macOS/Linux". The instructions below should update based on your selection. Copy the command listed for your operating system.

### Installing and Running Sireum Logika - Windows

For Windows users, open a File Explorer and navigate to `C:\Users\<yourAccountName>`. Select the text in the address bar like this:

![File Explorer](/images/fileExplorer.png)

Type "cmd" to overwrite the selected text:

![cmd](/images/cmd.png)

And hit Enter. It should open up a command prompt that is already in your `C:\Users\<yourAccountName>` folder:

![Open Command Prompt](/images/cmdOpen.png)

In the command prompt, paste the installer command you copied in the previous step. You should immediately see feedback that your computer is fetching Sireum Logika.

Once the installation is finished, open a file explorer and navigate to `C:\Users\<yourAccountName>\Applications\Sireum\bin\win\vscodium`. In that folder, you should see the application "CodeIVE.exe". Double-click that application to run it (if you see a popup that asks if you want to allow access to your computer, go ahead and choose that you want to do so).

You may find it handy to pin the "CodeIVE.exe" application to your taskbar. You can do so by right-clicking the "CodeIVE.exe" file and selecting "Pin to taskbar".

### Installation and Running Sireum Logika - Mac

For Mac users, open a terminal window and paste the command you copied in the previous step. You should immediately see feedback that your computer is fetching Sireum Logika.

Once the installation is finished, go to the finder and search for "CodeIVE". You should see the application "CodeIVE.exe" - if you run it, it will open Sireum Logika.

## How to Start Homework Assignments

To start a homework assignment (or to clone any existing repository, including homework solutions and lecture examples), first:
- Click to "accept the assignment" from the link in Canvas (if you are cloning a homework assignment or solution).
- Go to the URL created for your assignment (or to the URL of the repository you want to clone).
- You should be looking at a website that looks something like this:

![GitHub assignment](/images/gitHubAssign.png)

Click the Green *Code* button, so that you see something like this:

![GitHub clone](/images/gitHubClone.png)

Next, open Sireum VS Codium (by running the CodeIVE application). You should see:

![Open Sireum](/images/openSireum.png)

Next, click the third icon in the column on the left side (the one that looks like a circuit and says "Source Control" when you hover over it). You should see:

![Source Control](/images/openSourceControl.png)

Click *Clone Repository*. Now you should see something like:

 ![Clone Repository](/images/getFromVCS.png)

Paste the URL you copied from GitHub in the textbox indicated above. Click "Clone from URL". 

You will then be prompted to choose a folder to clone your repository into. Navigate to a folder on your computer specifically for CIS 301 (create one if it doesn't exist). Create a new empty folder within that CIS 301 folder to hold this new project. Select that new folder in the selection dialog, so that you now have something like this:

 ![Select Clone Location](/images/cloneToSireum.png)

Click *Select as Repository Destination*. You should see a popup that asks if you want to open the cloned repository. Select "Open". Next you should see something like:

![Clone Result](/images/clonedResult.png)

If you see a popup in the bottom right that asks if you want to import a Sireum project, then click *Yes*. 

## Installing Sireum Fonts

When you have Sireum VS Codium open, click the gear icon in the lower-right corner. Click the option that says "Command Palette":

![Open Command Palette](/images/openCommandPalette.png)

Type "run" in the resulting textbox. Click the option that says: "Tasks: Run Task".

![Run Task](/images/runTask.png)

Next, click the "sireum" folder:

![Sireum Task](/images/sireumTask.png)

Finally, click "sireum: --install-fonts":

![Install Fonts](/images/installFonts.png)

You might be asked for the installation location of Sireum at this point -- if so, it is `C:\Users\<yourAccountName>\Applications\Sireum`  for Windows and `/Users/<yourAccountName>/Applications/Sireum` for Mac.

## Running Logika Checker

When you are ready to verify a Logika truth table or proof, click the gear icon in the lower-right corner and select "Command Palette":

![Open Command Palette](/images/openCommandPalette.png)

Type "run" in the resulting textbox. Click the option that says: "Tasks: Run Task".

![Run Task](/images/runTask.png)

Next, click the "sireum logika" folder:

![Sireum Logika](/images/sireumLogika.png)

Finally, choose the option that says "sireum logika: verifier (file)". You should see either a popup that says "Logika verified", or you should see red error markings in your file indicating where the logic doesn't hold up. You can hover over these red error markings to get popups with more information on the error.

As a shortcut to verify a proof or truth table, you can do: `Ctrl-Shift-W` on Windows or `Command-Shift-W` on Mac.

NOTE: You might be asked to choose the installation folder of Sireum the first time you run the Logika checker. If you are, the installation folder is `C:\Users\<yourAccountName>\Applications\Sireum` for Windows and `/Users/<yourAccountName>/Applications/Sireum` for Mac.



## Committing and Pushing Changes

When you are finished working, commit and push your changes to GitHub. (I recommend doing this anytime you are at a stopping point, as well as when you are completely done.)

To do this, first save all your files (File-Save or Ctrl-S). Make sure none of the open file tabs have a solid circle next to the file name -- this is an indication that they are unsaved. When everything is saved, click the third icon in the column on the left side (the one that looks like a circuit and says "Source Control" when you hover over it). This is the same icon you clicked when cloning your repsitory.

Type in something in the "Message" box, and choose "Commit".

 ![Commit](/images/commit.png)

Next, click the button that says "Sync Changes" to push your commit to your remote repository:

 ![Push](/images/push.png)

If you go back to the URL of your GitHub repository and refresh the page, you should see your latest work.

Homework 0 will help you test the GitHub process. 

## Using Sireum in the Lab Classrooms

Sireum IVE (with Logika) is available in the CS computer lab classrooms (DUE 1114, 1116, and 1117). To find it, open a File Explorer, then open the C:\ drive. From there, navigate to "C:\Sireum\bin\win\vscodium" and run the "CodeIVE.exe" file. This will launch Sireum as usual.

If you want to complete your assignment using multiple computers, you can:
    - Clone the repo using the process above (if it is your first time working on the current assignment on this computer)
    - Commit/push your changes when you are ready to leave this computer
    - Do Git->Pull to get your latest changes if you come back to a different computer

## Remote Access

If you have any problems installing Sireum on your own computer, you can access it remotely from the CS department. See [here](https://support.cs.ksu.edu/CISDocs/wiki/Remote_Access) for information on remote access. When you connect to the department Window's server, you can follow the instructions above to find Sireum as you would on a lab computer.