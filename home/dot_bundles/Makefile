BREWFILE		= "$(abspath $(patsubst %/,%,$(dir $(abspath $(lastword $(MAKEFILE_LIST))))))/../.bundles/brewfile"
LOGFILE			= /tmp/$(id -un)-dotfiles.log

TARGET			= $(scutil --get ComputerName)
TAP 			= @echo tap  \"$(strip $(1))\" $(2) $(3) $(4) $(5)  >>$(BREWFILE)
BREW			= @echo brew \"$(strip $(1))\" $(2) $(3) $(4) $(5)  >>$(BREWFILE) 
CASK			= @echo cask \"$(strip $(1))\" $(2) $(3) $(4) $(5) 	>>$(BREWFILE) 
MAS				= @echo mas  \"$(strip $(1))\", id: $(2) 			>>$(BREWFILE) 

IS_ZSH 			= $$(basename $$SHELL) == "zsh"
IS_BASH 		= $$(basename $$SHELL) == "bash"
#@if [ $(IS_ZSH)  ]; then ...; fi
#@if [ $(IS_BASH) ]; then ...; fi

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

.DEFAULT_GOAL 	= help

.PHONY: clean

clean: ## Clean up things for a fresh make.
	rm -f $(BREWFILE) 

all: $(TARGET) ## Build all. Before use, please check if there is a goal for your computer name. 

FORCE:

# - - - -   B A S E L I N E   - - - - - - - - - - - - - - - - - - - - - - - - - - - -

etc-hosts: ## Apply a hosts file that blocks fakenews, gambling, porn and social malware. Needs root priviledges.
	sudo curl "https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews-gambling-porn-social/hosts" -o "/etc/hosts"
	sudo dscacheutil -flushcache;
	sudo killall -HUP mDNSResponder

macos-core: FORCE
	$(call TAP,  "homebrew/core")
	$(call TAP,  "homebrew/bundle")
	$(call TAP,  "homebrew/services")
	$(call TAP,  "homebrew/cask")
	$(call TAP,  "homebrew/cask-versions")
	
	@echo 'cask_args appdir: "~/Applications", require_sha: true' >>$(BREWFILE)
	
# 	GNU core utilities (those that come with macOS are outdated)
	$(call BREW, "coreutils") 
	@if [ $(IS_ZSH)  ]; then \
		echo 'export PATH="$$(brew --prefix coreutils)/libexec/gnubin:$$PATH"' >> ~/.zprofile; fi
	@if [ $(IS_BASH) ]; then \
		echo 'export PATH="$$(brew --prefix coreutils)/libexec/gnubin:$$PATH"' >> ~/.bash_profile; fi

linux-core:
	# tbd

baseline: 
	$(call BREW, "curl")				# download from internet
	$(call BREW, "git")					# source control
	$(call BREW, "bash")				# latest bash version
	$(call BREW, "bash-completion2")	# command completion for bash
	$(call BREW, "zsh")					# latest zsh version
	$(call BREW, "zsh-completions")		# command completion for zsh 

# - - - -   D E V E L O P   - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

fonts:
	$(call TAP,  "homebrew/cask-fonts")

	$(call CASK, "font-consolas-for-powerline")
	$(call CASK, "font-fira-code")
	$(call CASK, "font-hack")
	$(call CASK, "font-hack-nerd-font")
	$(call CASK, "font-roboto-mono")
	$(call CASK, "font-roboto-mono-for-powerline")

	$(call CASK, "font-inconsolata")
	$(call CASK, "font-inconsolata-g-for-powerline")
	$(call CASK, "font-inconsolata-lgc-nerd-font")

# nvm:
# 	$(call BREW, "nvm")

# nvm-node: nvm
# 	# Install latest stable node version
# 	nvm install node || exit 1

# python:
# 	# Fetch latest stable version. Ignore version 0.
# 	pyenv install "$$(pyenv install -l | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1
# 	pyenv global  "$$(pyenv install -l | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1

# pyenv:
# 	$(call BREW, "pyenv"
# 	@if [ $(IS_ZSH)  ]; then \
# 		$$(brew --prefix pyenv)/bin/pyenv init --path >> ~/.zprofile; \
# 		$$(brew --prefix pyenv)/bin/pyenv init - >> ~/.zshrc; fi
# 	@if [ $(IS_BASH) ]; then \
# 		$$(brew --prefix pyenv)/bin/pyenv init --path >> ~/.bash_profile; \
# 		$$(brew --prefix pyenv)/bin/pyenv init - >> ~/.bashrc; fi

# pyenv-python: pyenv
# 	# subshell now knows pyenv
# 	$(MAKE) python

# pyenv-virtualenv:
# 	$(call BREW, "pyenv-virtualenv"
# 	@if [ $(IS_ZSH)  ]; then \
# 		echo 'eval "$$(pyenv virtualenv-init -)"' >> ~/.zprofile; fi
# 	@if [ $(IS_BASH) ]; then \
# 		echo 'eval "$$(pyenv virtualenv-init -)"'>> ~/.bash_profile; fi 

# ruby:
# 	# Install latest stable ruby and bundler version. Ignore version 0.
# 	rbenv install "$$(rbenv install -l | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | tail -1)" | tee -a $(LOGFILE) || exit 1
# 	rbenv global  "$$(rbenv install -l | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | tail -1)" | tee -a $(LOGFILE) || exit 1
# 	gem install bundler || exit 1

# rbenv:
# 	$(call BREW, "rbenv")
# 	$(call BREW, "ruby-build")
# 	@if [ $(IS_ZSH)  ]; then \
# 		$$(brew --prefix rbenv)/bin/rbenv init --path >> ~/.zprofile; \
# 		$$(brew --prefix rbenv)/bin/rbenv init - >> ~/.zshrc; fi
# 	@if [ $(IS_BASH) ]; then \
# 		$$(brew --prefix rbenv)/bin/rbenv init --path >> ~/.bash_profile; \
# 		$$(brew --prefix rbenv)/bin/rbenv init - >> ~/.bashrc; fi

# ruby-gem: rbenv
# 	# subshell now knows rbenv
# 	$(MAKE) ruby

# rust:
# 	rustup-init | tee -a $(LOGFILE) || exit 1

# rustup:
# 	$(call BREW, "rustup")

# rust-cargo: rustup
# 	# subshell now knows rust
# 	$(MAKE) rust
	
# languages: pyenv-python pyenv-virtualenv nvm-node ruby-gem rust-cargo
# 	$(call BREW, "go", link: true, force: true)
# 	$(call BREW, "jq")
# 	$(call BREW, "pipx")

asdf:
	git clone https://github.com/asdf-vm/asdf.git ~/.asdf
	@if [ $(IS_ZSH)  ]; then \
		echo '. $$HOME/.asdf/asdf.sh' >> ~/.zprofile; fi
	@if [ $(IS_BASH)  ]; then \
		echo '. $$HOME/.asdf/asdf.sh' >> ~/.bash_profile; fi
		echo '. $$HOME/.asdf/completions/asdf.bash' >> ~/.bash_profile; fi


languages: asdf
	# json
	asdf plugin-add jq https://github.com/AZMCode/asdf-jq.git | tee -a $(LOGFILE) || exit 1
	# python plugin, python latest stable (ignore v0), latest stable python2 (ignore v0), global is latest + default python packages
	asdf plugin-add python | tee -a $(LOGFILE) || exit 1
	asdf install python "$$(asdf list all python | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1
	asdf install python "$$(asdf list all python | tr -d ' ' | grep '^2' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1
	asdf global python "$$(asdf list all python | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1
	# ruby plugin, ruby latest stable (ignore v0) + default gems (bundler)
	asdf plugin add ruby https://github.com/asdf-vm/asdf-ruby.git | tee -a $(LOGFILE) || exit 1
	asdf install ruby "$$(asdf list all ruby | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1
	asdf global ruby  "$$(asdf list all ruby | tr -d ' ' | grep '^[0-9]' | grep -v '\.0$$' | grep -v '[a-zA-Z]' | tail -1)" | tee -a $(LOGFILE) || exit 1
	# rust plugin, rust latest stable + default cargo packages
	asdf plugin-add rust https://github.com/code-lever/asdf-rust.git | tee -a $(LOGFILE) || exit 1
	asdf install rust stable | tee -a $(LOGFILE) || exit 1
	# nodejs plugin, nodejs latest stable + default npm packages
	asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git | tee -a $(LOGFILE) || exit 1
	asdf install nodejs latest | tee -a $(LOGFILE) || exit 1

git: languages
	$(call BREW, "gh")
	$(call BREW, "git-delta")
	$(call BREW, "jesseduffield/lazygit/lazygit")		# git ui (https://github.com/jesseduffield/lazygit)

	$(call CASK, "gitfinder")							# git finder integration
	$(call CASK, "github")
	$(call CASK, "kdiff3")
	$(call CASK, "p4merge")
#	$(call CASK, "sourcetree")

	go get -u github.com/nikitavoloboev/gitupdate

develop: fonts languages git ## Development toolset (Commandline tools, languages, git, fonts)
	$(call TAP, "buo/cask-upgrade")
	$(call TAP, "adoptopenjdk/openjdk")
	$(call TAP, "koekeishiya/formulae")
	$(call TAP, "cantino/mcfly")

	$(call BREW, "ack")					# grep replacement
	$(call BREW, "bat")					# cat replacement
	$(call BREW, "broot")				# tree replacement
	$(call BREW, "eth-p/software/bat-extras")	# batman, batpipe, batwatch, batdiff, prettybat
	$(call BREW, "binutils")				# gnu binary tools for native development
#	$(call BREW, "ccat") 				# →bat
	$(call BREW, "cmake")				# cross-platform make
	$(call BREW, "croc")				# safe and fast remote file copy
	$(call BREW, "diffutils")			# gnu file comparison
	$(call BREW, "direnv") 				# →funky
	$(call BREW, "exa") 				# ls replacement
	$(call BREW, "fasd")				# fast command line
	$(call BREW, "fd")					# find replcement
	$(call BREW, "ffmpeg")				# codec
	$(call BREW, "findutils")			# gnu find, xargs, locate
#	$(call BREW, "funky") 				# folder dependent loading of environment settings →direnv
	$(call BREW, "fzf")					# fuzzy find
	$(call BREW, "gawk")				# gnu awk
	$(call BREW, "gettext")				# gnu i18n and l10n library
	$(call BREW, "gnu-sed") 			# gnu stream editor
	$(call BREW, "gnu-tar")				# gnu file archiver
	$(call BREW, "gnu-time")			# gnu time
	$(call BREW, "gnu-which")			# gnu which
	$(call BREW, "gnupg")				# open pretty good privacy
	$(call BREW, "grep")				# gnu grep, egrep and fgrep
	$(call BREW, "gzip")				# gnu data compression
	$(call BREW, "highlight")			# code syntax highlighting
	$(call BREW, "httpie")				# curl&wget replacement
	$(call BREW, "imagemagick", --with-webp) #image manipulation 
	$(call BREW, "isacikgoz/taps/tldr")	# quick command input help (https://github.com/isacikgoz/tldr)
	$(call BREW, "inetutils")			# gnu utilities for networking
	$(call BREW, "languagetool")		# style and grammar checker
	$(call BREW, "libmagic")			# file(1) command
	$(call BREW, "lftp")				# file transfer
	$(call BREW, "make", "link:false") 	# make
	$(call BREW, "cantino/mcfly/mcfly")	# fly through your shell history
	$(call BREW, "moreutils")			# collection of newer unix tools
	$(call BREW, "mysql")				# open source relational database
	$(call BREW, "mysql-client")		# open source relational database connector
	$(call BREW, "neovim")				# vim replacement
	$(call BREW, "readline", "link:true") # gnu command-line editing library
	$(call BREW, "rename")				# perl-powered file rename script
	$(call BREW, "ripgrep")				# grep search
	$(call BREW, "rsync")				# backup
	$(call BREW, "sd")					# find and replace
	$(call BREW, "starship")			# fast command line prompt
	$(call BREW, "stow")				# file link
	$(call BREW, "tag")					# file attributes
	$(call BREW, "tesseract")			# ocr
	$(call BREW, "tesseract-lang")		# ocr
#	$(call BREW, "tmux")				# terminal emulator
#	$(call BREW, "tree") 				# →broot
	$(call BREW, "vim", "--with-override-system-vi") # vi style editor
	$(call BREW, "watchman")			# watch files and take action
	$(call BREW, "webp")				# web image format
	$(call BREW, "wget", "--with-iri")	# internet file retriever
	$(call BREW, "wp-cli")				# wordpress commands
	$(call BREW, "xz")					# data compression
#	$(call BREW, "z") 					# →zoxide
	$(call BREW, "zlib")				# lossless data-compression
	$(call BREW, "zoxide")				# smart change directory
	$(call BREW, "zplug")				# plugin manager for zsh

	$(call CASK, "adoptopenjdk8")		# JDK from the Java User Group (JUG)
	$(call CASK, "adoptopenjdk8-jre")	# JRE from the Java User Group (JUG)
#	$(call CASK, "alacritty") 			# →kitty
	$(call CASK, "drawio")				# drawing program
	$(call CASK, "hex-fiend")			# hex editor
	$(call CASK, "kitty")				# terminal
	$(call CASK, "local")				# web development
	$(call CASK, "openinterminal")		# finder toolbar app
	$(call CASK, "raspberry-pi-imager")	# raspberry pi development
	$(call CASK, "staruml")				# UML diagrams
	$(call CASK, "trailer")				# managing pull requests and issues on GitHub
	$(call CASK, "visual-studio-code")	# development environment
#	$(call CASK, "yed")

	$(call MAS,  "Amphetamine", "937984704")			# machine sleep manager
	$(call MAS,  "Better Rename 10", "1063663640")	# file renamer
	$(call MAS,  "Encode64", "1083012025")			# base 64 encryption / decryption
	$(call MAS,  "Table Tool", "1122008420")			# simple csv viewer
	$(call MAS,  "WhatSize", "413702125")			# file system space viewer
	$(call MAS,  "Xcode", "497799835")				# apple development environment
	$(call MAS,  "Yummy FTP", "492068728")			# file transfer manager

# - - - -   D E S K T O P   - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

office: ## Office tools like edit suites, readers, calculators
	$(call CASK, "coteditor")							# simple editor
	$(call CASK, "libreoffice")							# office suite
	$(call CASK, "numi")								# calculator and converter
	$(call CASK, "skim")								# pdf reader and note-taking app
	
	$(call MAS,  "Keynote", "409183694")				# presenting
	$(call MAS,  "Numbers", "409203825")				# calculating
	$(call MAS,  "Pages", "409201541")				# writing

quicklook:
	$(call CASK, "apparency")
#	$(call CASK, "epubquicklook") # only runs until Mojave
	$(call CASK, "qlcolorcode")
	$(call CASK, "qlimagesize")
	$(call CASK, "qlmarkdown")
	$(call CASK, "qlprettypatch")
	$(call CASK, "qlstephen")
	$(call CASK, "qlvideo")
	$(call CASK, "quicklook-csv")
	$(call CASK, "quicklook-json")
	$(call CASK, "quicklookase")
	$(call CASK, "suspicious-package")
	$(call CASK, "webpquicklook")

desktop: quicklook ## Desktop tools like editors, search, view and clipboard tools
	$(call CASK, "alfred")								# launch agent
	$(call CASK, "cheatsheet")							# shortcuts of applications
	$(call CASK, "clipgrab")							# downloads videos and audio from websites
	$(call CASK, "dozer")								# show and hide statusbar icons
	$(call CASK, "keka")								# compression / decompression
	$(call CASK, "maccy")								# clipboard manager

#	$(call MAS,  "CopyLess", "416439303")				# clipboard manager
	$(call MAS,  "GoodLinks", "1474335294")				# link manager
	$(call MAS,  "HazeOver", "430798174")				# dim windows not in focus
	$(call MAS,  "HotKey App", "975890633")				# system wide hotkeys
	$(call MAS,  "Lightshot Screenshot", "526298438")	# screenshots
	$(call MAS,  "Magnet", "441258766")					# pin and size windows
	$(call MAS,  "PopClip", "445189367")				# clipboard helper
	$(call MAS,  "QREncoder", "452695239")				# generate qr codes
	$(call MAS,  "Trickster", "509641724")				# quick access to newly changed files

# - - - -   O N L I N E   - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

browser:
	$(call CASK, "brave-browser")
	$(call CASK, "firefox")
	$(call CASK, "google-chrome")

messenger:
	$(call CASK, "signal")
	$(call CASK, "telegram")
	$(call CASK, "whatsapp")

	$(call MAS,  "Twitter", "409789998")

collaboration:
	$(call CASK, "jitsi-meet")
#	$(call CASK, "slack")
	$(call CASK, "zoom")

online: browser messenger collaboration ## Online tools like browser, messenger and collaboration programs
	$(call CASK, "enpass")
	$(call CASK, "onedrive")
	$(call CASK, "transmission")

	$(call MAS,  "Faxbot", "640079107")
	$(call MAS,  "NordVPN", "905953485")
	$(call MAS,  "Telephone", "406825478")

# - - - -   P E R S O N A L   - - - - - - - - - - - - - - - - - - - - - - - - - - - -

3dprint:
	$(call CASK, "blender")
	$(call CASK, "autodesk-fusion360")
	$(call CASK, "freecad")
	$(call CASK, "meshmixer")
	$(call CASK, "openscad")
	$(call CASK, "prusaslicer")
	$(call CASK, "superslicer")
	$(call CASK, "ultimaker-cura")

audio:
	$(call CASK, "kid3")
	$(call CASK, "mp3tag")
	$(call CASK, "musicbrainz-picard")
	$(call CASK, "spotify")
	$(call CASK, "tagger")
	$(call CASK, "xld")
	$(call CASK, "yate")

	$(call MAS,  "AudioBookBinder", "413969927")

foto:
	$(call CASK, "affinity-designer")
	$(call CASK, "affinity-photo")
	$(call CASK, "affinity-publisher")
	$(call CASK, "google-nik-collection")
	$(call CASK, "imageoptim")
	$(call CASK, "vuescan")

	$(call MAS,  "Pikka", "1195076754")
	$(call MAS,  "Pixelmator", "407963104")

video:
	$(call CASK, "boom-3d")
	$(call CASK, "burn")
	$(call CASK, "handbrake")
	$(call CASK, "makemkv")
	$(call CASK, "mediathekview")
	$(call CASK, "mpeg-streamclip")
	$(call CASK, "subler")
	$(call CASK, "tv-browser")
	$(call CASK, "vidcutter")
	$(call CASK, "videospec")
	$(call CASK, "vlc")

	$(call MAS,  "MediaInfo", "510620098")
	$(call MAS,  "Vinegar - Tube Cleaner", "1591303229")

media: audio foto video

outdoor:
	$(call BREW, "gpsbabel")

	$(call CASK, "routeconverter")

	$(call MAS,  "Garmin BaseCamp", "411052274")
	$(call MAS,  "Garmin POILoader", "411695198")
	$(call MAS,  "Garmin WebUpdater", "477318609")
	$(call MAS,  "GPX Editor", "924782627")
	$(call MAS,  "myOSM", "413972659")

evaluate:
	$(call CASK, "tagspaces")

wolfgang: evaluate ## Include Wolfgangs private tool selection
	$(call CASK, "jdownloader")
	$(call CASK, "joplin")
	$(call CASK, "macdown")
	$(call CASK, "moneymoney")
	$(call CASK, "qgis")
	$(call CASK, "typinator")

	$(call MAS,  "Brother P-touch Editor", "1453365242")
	$(call MAS,  "Gifski", "1351639930")
	$(call MAS,  "Mactracker", "430255202")
	$(call MAS,  "Pricepirates", "461727159")

# - - - -   S Y S T E M   - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

backup:
#	$(call CASK, "arq"
	$(call CASK, "backuploupe")
	$(call CASK, "carbon-copy-cloner")
	$(call CASK, "chronosync")
	$(call CASK, "duplicacy")
	$(call CASK, "duplicacy-web-edition")
	$(call CASK, "hyperbackupexplorer")

security:
	$(call BREW, "openssh")

	$(call CASK, "knockknock")
	$(call CASK, "lulu")
	$(call CASK, "silentknight")

system: backup security ## Inlcude system tools for backup and security
	$(call CASK, "appcleaner")			# delete an application completely
	$(call CASK, "connectmenow"	)		# establish smb connections
	$(call CASK, "filezilla")			# ftp
#	$(call CASK, "grandperspective")	# disk size usage
#	$(call CASK, "istat-menus")			# task bar system status
#	$(call CASK, "iterm2")				# terminal replacement
	$(call CASK, "launchcontrol")		# system services toolbar
	$(call CASK, "monolingual")			# remove unnecessary language resources
	$(call CASK, "sloth")				# displays all open process resources
	$(call CASK, "taskexplorer")		# explore all running tasks
	$(call CASK, "the-unarchiver")		# file decompression

	$(call MAS,  "Cleaner-App", "1013897218")	# optimize disk space usage

# - - - -   P L A T F O R M S   - - - - - - - - - - - - - - - - - - - - - - - - - - -

macos-settings: macos_settings.sh ## Apply default MacOS settings
	$$(SHELL) macos_settings.sh		# reviewed default user settings for macos

macos:	macos-core etc-hosts macos-settings baseline ## Include basic set of programs, default settings and safe hosts file for MacOS
	$(call BREW, "mackup")				# backup plist of well-known programs
	$(call BREW, "mas")					# appstore upgrades via commandline
	$(call BREW, "topgrade")			# system wide update process

linux:	linux-core etc-hosts baseline ## Include basic set of programs for Linux 
	# tbd

# - - - -   T A R G E T S   - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

MacW:	macos desktop office online develop media outdoor 3dprint system wolfgang $(BREWFILE) ## Target machine MacW
	@echo brew bundle --file=$(BREWFILE)

MacC:	macos desktop office online $(BREWFILE) ## Target machine MacC
	@echo brew bundle --file=$(BREWFILE)

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

.PHONY: help

# Check http://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
help: ## Print this help
	#@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

ignore:
	$(call CASK, "avidemux")
	$(call CASK, "boxcryptor")
	$(call CASK, "chromedriver")
	$(call CASK, "db-browser-for-sqlite")
	$(call CASK, "disk-sensei")
	$(call CASK, "evernote")
	$(call CASK, "folx")
	$(call CASK, "free")
	$(call CASK, "garagebuy")
	$(call CASK, "google-earth-pro")
	$(call CASK, "kap")
	$(call CASK, "keepassxc")
	$(call CASK, "krisp")
	$(call CASK, "macx-dvd-ripper-pro")
	$(call CASK, "notion")
	$(call CASK, "openshot-video-editor")
	$(call CASK, "postman")
	$(call CASK, "softorino-youtube-converter")
	$(call CASK, "sublime-text")
	$(call CASK, "vagrant")
	$(call CASK, "vnc-viewer")
	$(call CASK, "waltr")
	
	$(call MAS,  "1Password", "443987910")
	$(call MAS,  "Agenda", "1287445660")
	$(call MAS,  "AmorphousDiskMark", "1168254295")
	$(call MAS,  "Blackmagic Disk Speed Test", "425264550")
	$(call MAS,  "Covered", "860548294")
	$(call MAS,  "Dato", "1470584107")
	$(call MAS,  "Duplicate File Finder Remover", id:1032755628")
	$(call MAS,  "Duplicate File Finder", "1032755628")
	$(call MAS,  "Exporter", "1099120373")
	$(call MAS,  "ForkLift", "412448059")
	$(call MAS,  "Free", "486226904")
	$(call MAS,  "Gapplin", "768053424")
	$(call MAS,  "GIPHY CAPTURE", "668208984")
	$(call MAS,  "Gladys", "1382386877")	
	$(call MAS,  "Growl", "467939042")
	$(call MAS,  "HotKey", "975890633")
	$(call MAS,  "LiveReload", "482898991")
	$(call MAS,  "Microsoft OneNote", "784801555")
	$(call MAS,  "Microsoft Remote Desktop", "715768417")
	$(call MAS,  "Monity", "915542151")
	$(call MAS,  "Reeder 5." "1529448980")
	$(call MAS,  "Remote Desktop", "409907375")
	$(call MAS,  "Speedtest", "1153157709")
	$(call MAS,  "TidyMyMusic", "698541606")
	$(call MAS,  "Total Manager", "796275163")
