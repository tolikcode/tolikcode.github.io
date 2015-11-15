+++
date = "2015-11-15T11:12:12+02:00"
title = "Taking the trash out with PowerShell"

+++

Many Windows users tend to store all of their old unnecessary files in the Recycle Bin. However, it just irritates me to know that files I almost definitely won't ever need are still taking space up on my hard drive. Immediately deleting these files bypassing the Recycle Bin is also not an option here.

<!--more-->

## Goal
Recycle Bin should be something like a safety net in case of mistakenly deleted files and pretend it doesn't even exist otherwise. Windows automatically empties your Recycle Bin when it reaches certain size limit. In my opinion there should be a time limit additionally to the size limit in the Recycle Bin. 
There are third party applications that might help you with it. But people like me who are irritated by three month old files in their Recycle Bins are probably not going to be very happy with an application cluttering their computers just for this simple task. Fortunately you can do it with PowerShell.

## Solution
After some googling I've found a detailed answer from `Indrek` on [SuperUser](http://superuser.com/a/434673). I liked the answer so much I decided to copy it here. It's also a nice opportunity to learn a bit of PowerShell.

~~~
ForEach ($Drive in Get-PSDrive -PSProvider FileSystem) {
    $Path = $Drive.Name + ':\$Recycle.Bin'
    Get-ChildItem $Path -Force -Recurse -ErrorAction SilentlyContinue |
    Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-30) } |
    Remove-Item -Recurse
}
~~~

You can find a line-by-line explanation of this script in the original post from `Indrek`. The script iterates over your disks, checks Recycle Bin folders and deletes all files that were put there more than some predefined period of time ago (in this case 30 days).

Save this script as a text file with a `.ps1` extension. You can then use Task Scheduler to run this at regular intervals. Open Task Scheduler and create a new task with the following parameters:

1. Under the "General" tab, enter a name and check the "Run with highest privileges" option.
2. Under the "Triggers" tab, add a new trigger and set the task to run daily.
3. Under the "Actions" tab, add a new action:
  - set the "Program/script" field to `powershell`
  - set the "Add arguments" field to `-NonInteractive -NoProfile -ExecutionPolicy RemoteSigned -File "C:\path\to\script.ps1"`

I've been using this solution for several months already and it works like a charm.
