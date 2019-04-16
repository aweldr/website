---
title: "Homebrew Install"
date: 2019-04-15T18:38:25-04:00
draft: False
---
<h2> by Matthew Barlowe</h2>
      <br />
      <p>Homebrew is a very usefual package manager that can be installed on the macOS system.
      It's what I use to manage by Python installations along with my SQL database as well.
      It has a nice, easy to learn, syntax and is simple to install from the terminal on an
      Apple computer. What is a package manager you ask? Well join me in the next section and
      we'll discuss the details behind it.</p>
      <h2>What is a Package Manager?</h2>
      <p>A package manger is according to <a href='https://en.wikipedia.org/wiki/Package_manager' target="_blank">Wikipedia</a>
      "A a collection of software tools that automates the process of installing, upgrading,
      configuring, and removing computer programs for a computer's operating system in a
      consistent manner." In normal speak its just a program that handles programs on your>Wikipedia</a>
      "A a collection of software tools that automates the process of installing, upgrading,
      configuring, and removing computer programs for a computer's operating system in a
      consistent manner." In normal speak its just a program that handles programs on your
      computer. Instead of having to go to Google to upgrade Chrome, or going to Adobe to
      upgrade Photoshop, this program takes care of all that leg work and brings the files to
      you while all you have to do is type a simple command.</p>
      <p>The package manager will also keep track of what programs you have installed on your
      system and what versions of those programs are installed as well. It's a very useful for
      getting things up and running quickly and effortlessly without having to worry about dealing
      with source files. If I was powering on a new Macbook or iMac for the first time, Homebrew
      would be the first program I installed to get the software I need to develop my scripts.
      <h2>Installing Homebrew</h2>
      <p>Installing Homebrew is simple and just takes this simple one line command:
      <br />
      <br />
        <code>
          /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
        </code>
      <br />
      <br />
      Copy and paste that into your terminal and hit enter and that's all you need to do.
      And now you're on your way to becoming a programmer. You can read more about the Homebrew
      package manager at the <a href='https://brew.sh' target="_blank">Homebrew website.</a>
      <h2>How Does Homebrew Work?</h2>
      <p>If you clicked on the page I linked above you would see that "Homebrew installs packages
      to their own directory and then symlinks their files into <code>/usr/local</code>. Ok, so what
      does all that mean? We'll break it down piece by piece. So first Homebrew installs the files
      you want in its own file system so they don't contaminate other files of the operating system.
      This is especially important for langauges like Ruby and Python where the Mac OS has system
      installed versions of these langauges already installed in <code>/usr/bin</code>. You never want
      to touch the versions of these located in that folder as it can affect other programs the OS
      runs.</p>
      <p>But the operating system won't know where these files are located because the directory is
      not stored in the OS <code>$PATH</code> environment variable(If you don't understand what that is
      don't worry we'll get there in future tutorials). So to get around that Homebrew creates a symbolic
      link to its files and stores those in your <code>/usr/local/bin</code> which is in the <code>$PATH</code>
      variable.</p>
      <p>A symbolic link is basically the same as a shortcut in Windows or Mac where you create an
      icon on your desktop to point to a file or a directory. However, a symbolic link does a little
      bit more as it <a href='https://www.makeuseof.com/tag/what-is-a-symbolic-link-what-are-its-uses-makeuseof-explains/' target="_blank">
        "will make it look like the linked file is actually there, rather than it just being a shortcut."</a>
      So Homebrew is making it look like the programs are in the bin folder
      so when we type the commands into terminal they will execute properly.</p>
      <p>One last thing I'll touch on quickly is why installing to <code>/usr/local/</code>
      is important.  <code>/usr</code> back in the olden days was used to store executables and
      libraries that were not system critical, and <code>/usr/local/</code> was used to store software
      that wouldn't be overwritten with a system update i.e. it was installed locally. However that
      has changed in recent years with most linuxes symbolic linking <code>/bin</code> to
      <code>/usr/bin</code> and Apple has <code>/usr</code> protected as well. By installing into
      <code>/usr/local/</code>we avoid accidentally overwritting or messing up files in our
      <code>usr/bin</code> that could cause serious issues in the operation of the Mac OS. Again
      it's not neccesary to know any of this to get Homebrew to work on your computer so if you don't
      I wouldn't worry about it too much.
      <h2>Homebrew Commands</h2>
      <p>Homebrew has three basic commands:
      <br />
      <br />
      <code>brew install [package name]</code>
      <br />
      <br />
      <code>brew uninstall [package name]</code>
      <br />
      <br />
      <code>brew upgrade [package name]</code>
      <br />
      <br />
      These three commands will install new packages, remove old packages you no longer want,
      and update packages that are out of date. That's it, pretty easy right? There's a lot more
      to Homebrew than this, but if you want to learn more I suggest checking out the documentation
      in the link to their site above or the links in the sources I list below.
      <h2>Other OS Package Managers:</h2>
      <p>Homebrew is only available for the MacOS, but if you're on Windows or Linux don't despair.
      <a href='http://linuxbrew.sh'>Linux Brew</a> is a package manager for Linux that is a fork of
      the Homebrew repo so should work in a very similar fashion. There's also more traditional linux package
      managers such as `yum` and `apt`.
      For Windows there seems to be
      <a href='https://chocolatey.org'>Chocolatey</a> that has both a GUI and a command line
      implementation.  I hav used `linuxbrew` for a little bit but it didn't work quite as
      seemlessly as Homebrew does for OS X so I'd recommend going with `yum` or `apt`.
      As far as Chocolatey I haven't used it at all so I can't speak to its usefulness.
      <a href='mailto:mcbarlowe@gmail.com'>mcbarlowe@gmail.com</a> and let me know.</p>
      <h2>Sources:</h2>
      <p><a href='http://www.pyladies.com/blog/Get-Your-Mac-Ready-for-Python-Programming/'>
        Getting Your Mac Ready For Python Programming</a>
      <br />
      <a href='https://kb.iu.edu/d/abbe' target="_blank">Create a Symbolic Link in Unix</a>
      <br />
      <a href='https://unix.stackexchange.com/questions/4186/what-is-usr-local-bin' target="_blank">What is /usr/local/bin</a>
      <br />
      <a href='https://en.wikipedia.org/wiki/Unix_filesystem#Conventional_directory_layout' target="_blank">Unix Filesystem Layout</a>
      <br />
      <a href='https://www.centos.org/docs/5/html/5.1/Deployment_Guide/s3-filesystem-usr-local.html' target="_blank">CentOS Docs</a>
      <br />
      <a href='https://en.wikipedia.org/wiki/PATH_(variable)' target="_blank">$PATH variable</a>
      <br />
      <a href='https://computers.tutsplus.com/tutorials/homebrew-demystified-os-xs-ultimate-package-manager--mac-44884' target="_blank">Homebrew Demystified</a>
