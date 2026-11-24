---
title: Three ways to back up Windows 11 to an external drive
seo_title: Back up Windows 11 to an external drive: three built-in ways
description: Learn how to back up Windows 11 to an external drive with File History, Backup and Restore, or File Explorer, and what each one copies.
date: 2026-11-24T20:40:00
updated: 2026-11-24T20:40:00
category: pc-storage
desk: pc
image: https://tse3.mm.bing.net/th?q=how+to+back+up+windows+11+to+an+external+drive&w=820&h=462&c=7&rs=1&p=0&o=5&pid=1.7
image_alt: External hard drive connected to a laptop on a wooden desk with a coffee mug nearby
source: contentforge
cf_id: 11878
---
<div class="key"><b>In short:</b> Use File History for personal files if the drive is NTFS. If the drive is exFAT and you can&#x27;t reformat, use File Explorer to copy folders manually. File History is the best default for home users because it runs on a schedule and keeps versions. Windows 11&#x27;s built-in image tool is deprecated and unmaintained. File History and a manual copy do not copy Windows itself.<ul><li>File History copies Libraries, Desktop, Contacts and Favorites, not your whole PC.</li><li>A manual File Explorer copy works on any drive format.</li><li>Verify by comparing file counts and total size in Properties, then open a few files from the drive.</li><li>Leave room for at least a full second copy of your data, so File History&#x27;s versions have space to grow.</li></ul></div>
<h2>Which built-in Windows 11 tools exist and what each copies</h2>
<p>Windows 11 ships with three ways to back up to an external drive. They are not equal. Each one copies a different set of files and expects a different drive format. Windows Backup, the app in Settings, backs up to OneDrive rather than a drive, so it is outside this guide.</p>
<p>File History copies your personal folders only: Documents, Pictures, Music, Videos, Desktop, Contacts and Favorites. It runs on a schedule and keeps versions, so you can roll a file back to yesterday.</p>
<p>Backup and Restore (Windows 7) is still in Windows 11 for compatibility. It can make a system image and a file backup. Microsoft&#x27;s own help page describes it as deprecated, which means the company does not develop it further.</p>
<p>A manual copy in File Explorer is the simplest route. You pick the folders, drag them to the drive, and wait. Nothing is scheduled and nothing is versioned, but you see every file that goes across.</p>
<table><caption>The three built-in options compared</caption><thead><tr><th>Tool</th><th>What it copies</th><th>Drive format</th><th>Automatic</th></tr></thead><tbody><tr><td>File History</td><td>Libraries, Desktop, Contacts, Favorites</td><td>NTFS only</td><td>Yes, on a schedule</td></tr><tr><td>Backup and Restore (Windows 7)</td><td>System image and file backup</td><td>NTFS</td><td>Yes, on a schedule</td></tr><tr><td>Manual File Explorer copy</td><td>Whatever you select</td><td>Any format</td><td>No</td></tr></tbody></table>
<div class="note warn"><b>Warning:</b> File History and manual copy do not back up Windows itself. If you want a full-system image, use a third-party tool.</div>
<table><thead><tr><th>At a glance</th><th>Answer</th></tr></thead><tbody><tr><td>Built-in tools</td><td>3 options</td></tr><tr><td>System image</td><td>Not included</td></tr><tr><td>File History format</td><td>NTFS only</td></tr><tr><td>Manual copy format</td><td>Any format</td></tr><tr><td>First backup time</td><td>Depends on USB speed and file count</td></tr><tr><td>Later incremental runs</td><td>Minutes, not hours</td></tr><tr><td>3-2-1 rule</td><td>3 copies, 2 media, 1 offsite</td></tr></tbody></table>
[[tool:download-time-calculator]]
<h2>The built-in system image tool is deprecated and unmaintained</h2>
<p>That leaves the image tool as a fallback, not a plan. It still works for many setups, but you get no fixes if it breaks.</p>
<p>What this means for you: keep your personal files backed up, and keep install media ready for a reinstall. You can make it with the Media Creation Tool from Microsoft.</p>
<p>A clean install on an SSD is usually the short part; reinstalling apps and drivers is the real time sink. If you need a restoreable image, third-party tools exist. AOMEI Backupper Standard and Hasleo Backup Suite Free are current options. Check their own documentation for Windows 11 support.</p>
<div class="note warn"><b>Common mistake:</b> A system image from a deprecated tool is a fallback, not a plan. If you rely on one, test that it restores before you need it.</div>
<h2>OneDrive Files On-Demand placeholders look like files but are not on the drive</h2>
<p>OneDrive Files On-Demand shows your cloud files in File Explorer with a cloud icon. They look present. They are not on your disk. If you copy that folder to an external drive, some files may come across as tiny placeholder stubs, or fail to copy at all.</p>
<p>The fix is to right-click the OneDrive folder in File Explorer, choose &quot;Always keep on this device&quot;, and wait for the download to finish. The icon changes to the solid green circle that means always kept on this device. Wait for it before copying. Then read the folder&#x27;s size in Properties on both sides, and only start the backup when the sizes match.</p>
<p>Check the size before and after. A folder showing 2 GB in OneDrive but 40 MB on the external drive means most of it is still in the cloud. That size gap is the giveaway that a backup only looks complete.</p>
<p>Green tick means downloaded. A solid green circle means always available locally. A blue double arrow means the item is shared with someone else, not that it is online-only. Learn those three icons and you&#x27;ll spot the problem in seconds.</p>
<ul><li>Blue cloud icon: online only, not on your disk.</li><li>Green tick: downloaded, safe to copy.</li><li>Solid green circle: always kept on this device.</li><li>Yellow warning: sync problem, fix before backing up.</li></ul>
<h2>exFAT versus NTFS on the external drive and what each costs you</h2>
<p>The format of your external drive decides which tools will run. NTFS is Windows&#x27; native format. It supports files larger than 4 GB and keeps permissions.</p>
<p>exFAT works on Windows, Mac and Linux. It has no 4 GB file limit, and it&#x27;s the usual choice for USB sticks that travel between machines. The cost: File History will not run on it, and exFAT has no journal, so a power loss or an unplug during a write is more likely to corrupt the file system.</p>
<p>FAT32 is the oldest of the three. It caps any single file at 4 GB, which rules out most modern video files and disk images. Windows 11 can read FAT32 but the built-in format tool won&#x27;t create it above 32 GB. Avoid it for backups.</p>
<p>You can check the current format by right-clicking the drive in File Explorer, choosing Properties, and reading the File system line. Reformatting erases the drive, so move anything you want to keep first.</p>
<table><caption>External drive formats and what they allow</caption><thead><tr><th>Format</th><th>Max file size</th><th>File History</th><th>Mac and Linux</th></tr></thead><tbody><tr><td>NTFS</td><td>Very large (practical limit is disk size)</td><td>Yes</td><td>Read-only on Mac by default</td></tr><tr><td>exFAT</td><td>Very large</td><td>No</td><td>Yes</td></tr><tr><td>FAT32</td><td>4 GB per file</td><td>No</td><td>Yes</td></tr></tbody></table>
<div class="note"><b>Rule of thumb:</b> An exFAT drive you cannot reformat still works for a manual copy. You lose automatic scheduling but gain cross-platform compatibility.</div>
<h2>How to back up Windows 11 to an external drive step by step</h2>
<p>Plug the drive in and give Windows a few seconds to mount it. Open File Explorer with Windows key plus E. The drive shows under This PC with a letter, usually D or E.</p>
<p>If the drive doesn&#x27;t appear with a letter, open Disk Management by searching for it in the Start menu. Look for the drive in the lower pane. If it shows Unallocated, right-click and choose New Simple Volume. If it shows a volume but no letter, right-click the volume and choose Change Drive Letter and Paths, then Add.</p>
<p>For File History: open Settings, then System, then Storage, then Advanced storage settings, then Backup options. Or search File History in the Start menu. Choose your external drive and turn it on. The first run depends on link speed and file count. A USB 3.0 drive with mostly large files moves far faster than one full of small files.</p>
<p>For a manual copy: open a second File Explorer window, navigate to your user folder under C:\Users\YOUR_NAME, and drag the folders you want across. Skip AppData unless you know which app settings you need. Skip the Windows folder entirely.</p>
<p>Then verify. Right-click your source folder, choose Properties, and note the size and file count. Do the same on the copy. If the source shows 47,382 files and 112 GB and the copy shows 47,380 files and 112 GB, you&#x27;re within rounding. Open three or four files from the drive to confirm they open, not just that they exist.</p>
<ol class="steps"><li>Plug the drive in. Confirm it shows under This PC.</li><li>Right-click the drive, open Properties, and read the File system line.</li><li>Pick your route: File History, or a manual drag-and-drop copy.</li><li>For File History, use Settings, then System, then Storage, then Backup options, and choose the drive.</li><li>Wait until the progress window closes, then compare file counts and open a few files.</li></ol>
<div class="note"><b>Check this:</b> Keep the drive plugged in until the copy finishes and you&#x27;ve verified the file count. Don&#x27;t unplug until the progress window closes and you have compared file counts.</div>
<h2>How much space to leave free on the backup drive</h2>
<p>Don&#x27;t fill the backup drive to the top. File History writes new versions each run, and a full drive stalls the next backup without warning. File History stores multiple versions, so leave room for at least a full second copy of your data.</p>
<p>A 1 TB external drive backing up 400 GB of files has room to grow. A 500 GB drive backing up 480 GB will fail the next time a large file changes. Windows won&#x27;t stop you from filling it, but the backup will quietly stop working.</p>
<p>Don&#x27;t use the backup drive for other storage. A drive that holds both your backup and your movie collection is not a backup. It&#x27;s just another place your files live, and you lose it all in one event.</p>
<p>Check the drive&#x27;s free space once a month. Right-click the drive in File Explorer, choose Properties, and read the Free space line. If it&#x27;s below 30 percent, either delete old versions in File History settings or move to a larger drive.</p>
<h2>The 3-2-1 check for a drive that gets unplugged</h2>
<p>A single external drive is not a backup on its own. It is one copy. Standards bodies and recovery firms use the 3-2-1 rule: 3 copies of your data, on 2 different types of media, with 1 copy offsite.</p>
<p>Your laptop holds copy one. The external drive is copy two, on different media. Copy three is the one that survives a fire, a theft or a ransomware attack that encrypts both machines. That copy lives in the cloud or at another address.</p>
<p>For everyday files, a free cloud tier covers your most important documents. Free tiers hover in the single-digit-to-low-double-digit GB range; check the provider&#x27;s current plan. For photos and video, a paid tier or a second drive kept at a relative&#x27;s house does the job.</p>
<p>One more check: an external drive that stays plugged in and powered up all year is not a backup either. A power surge or a ransomware encryption pass can take both the PC and the drive. Unplug it when you&#x27;re done.</p>
<ul><li>Copy 1: your laptop&#x27;s internal drive.</li><li>Copy 2: the external drive, unplugged when not in use.</li><li>Copy 3: cloud or a second drive at another address.</li><li>Test a restore once a year. A backup you never try to restore is a guess.</li></ul>
<div class="note"><b>Our take:</b> For most people: turn on File History to an NTFS drive, and keep a Windows install USB on the shelf for reinstalls.</div>
<h2>Quick answers</h2>
<h3>Can I use File History on an exFAT external drive?</h3>
<p>No. File History only runs on an NTFS drive. With an exFAT drive you cannot reformat, copy the folders by hand in File Explorer instead.</p>
<h3>Does Windows 11 have a system image backup built in?</h3>
<p>Not a current one. Backup and Restore (Windows 7) is still present but marked deprecated by Microsoft, meaning it gets no further development. For a restoreable image you need third-party software.</p>
<h3>How long does the first backup take?</h3>
<p>It depends on USB speed and file count. A USB 3.0 drive with large files moves faster than USB 2.0 with thousands of small files. Later incremental runs take minutes, not hours.</p>
<h3>Why does my copied OneDrive folder look smaller on the external drive?</h3>
<p>Files On-Demand left most files as placeholders in the cloud. Right-click the OneDrive folder, choose Always keep on this device, and wait for the downloads to finish before copying again.</p>
<h3>Is one external drive enough for a backup?</h3>
<p>No, if it&#x27;s your only copy. The 3-2-1 rule asks for three copies, two kinds of media, one offsite. A single external drive left plugged in can be lost to the same event that takes the laptop.</p>
<h3>What if Windows doesn&#x27;t show a drive letter for my external drive?</h3>
<p>Open Disk Management from the Start menu. Find the drive in the lower pane. If it shows Unallocated, right-click it and pick New Simple Volume to create a partition. If it shows a volume but no letter, right-click and choose Change Drive Letter and Paths, then Add.</p>
