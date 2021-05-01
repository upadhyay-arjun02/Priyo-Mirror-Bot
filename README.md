[![Priiiyo](https://telegra.ph/file/9f79dea91ab7cda63dc46.jpg)](http://t.me/PriiiyoMirror)

# Priiiyo Mirror Bot
A Telegram bot which can mirror all your links to Google drive! This is the Modified version.

# Features supported:

## From Source Repos
- Mirroring direct download links, Torrent, and Telegram files to Google Drive
- Mirroring Mega.nz links to Google Drive (In development stage)
- Copy files from someone's drive to your drive (Using Autorclone)
- Download/upload progress, speeds and ETAs
- Mirror all youtube-dl supported links
- Docker support
- Stable Mega.nz support
- Uploading To Team Drives
- Index Link support
- Service account support
- Delete files from drive
- Shortener support
- Custom Filename (Only for url, telegram files and ytdl. Not for mega links and magnet/torrents)
- Extracting password protected files, using custom filename and download from password protected index links see these examples:
<p><a href="https://telegra.ph/Magneto-Python-Aria---Custom-Filename-Examples-01-20"> <img src="https://img.shields.io/badge/see%20on%20telegraph-grey?style=for-the-badge" width="190""/></a></p>

- Extract these filetypes and uploads to google drive
```
ZIP, RAR, TAR, 7z, ISO, WIM, CAB, GZIP, BZIP2, 
APM, ARJ, CHM, CPIO, CramFS, DEB, DMG, FAT, 
HFS, LZH, LZMA, LZMA2, MBR, MSI, MSLZ, NSIS, 
NTFS, RPM, SquashFS, UDF, VHD, XAR, Z.
```


## Additional Features
- Mirroring Uptobox.com links to Google Drive (Uptobox account must be premium)
- Nyaa.si and Sukebei Torrent search
- Speedtest with picture results
- Limiting torrent size support
- Check Heroku dynos stats
- Add stickers to your pack
- Shell and Executor


## Special Features

- Dynamic Config Support, to facilitate easier and streamlined experience for editing config files.
- Sync Config Files at every /restart command.
- Edit values of environment variables in 'config.env' from within the bot using InlineKeyboardButtons.
- 'aria2c' daemon starts as a subprocess from within the 'bot' python module, facilitating better handling of processes. This also results in both 'aria2c' daemon and the 'bot' python module restarting with every '/restart' command.
- Support for using custom tracker list formatted as a text file, as required by 'aria2c' daemon.


# Development Status:

## To-Dos

- Add option to select files for torrent/magnet downloads.

**NOTE:** All the above to-dos are aimed at achieving zero human-intervention after initial deploy to Heroku.

## Info of Branches

- **master** : most stable environment for production deploys.
- **beta** : testing new features, fixes or better implementations of already existing ones.
- **dev** : major feature updates that are under development - currently, added support for 'mega-dl'.


# How to deploy?
Deploying is pretty much straight forward and is divided into several steps as follows:

## Installing requirements

- Clone this repo:
```
git clone https://github.com/priiiyo/priiiyo-mirror-bot mirrorbot/
cd mirrorbot
```

- Install requirements
For Debian based distros
```
sudo apt install python3
sudo snap install docker 
```
- For Arch and it's derivatives:
```
sudo pacman -S docker python
```

## Setting up config file
<details>
    <summary><b>Click here for more details</b></summary>

```
cp config_sample.env config.env
```
- Remove the first line saying:
```
_____REMOVE_THIS_LINE_____=True
```
Fill up rest of the fields. Meaning of each fields are discussed below:
- **BOT_TOKEN**: The telegram bot token that you get from [@BotFather](https://t.me/BotFather)
- **GDRIVE_FOLDER_ID**: This is the folder ID of the Google Drive Folder to which you want to upload all the mirrors.
- **DOWNLOAD_DIR**: The path to the local folder where the downloads should be downloaded to
- **DOWNLOAD_STATUS_UPDATE_INTERVAL**: A short interval of time in seconds after which the Mirror progress message is updated. (I recommend to keep it 5 seconds at least)  
- **OWNER_ID**: The Telegram user ID (not username) of the owner of the bot
- **AUTHORIZED_CHATS**: Fill user_id and chat_id of you want to authorize.
- **AUTO_DELETE_MESSAGE_DURATION**: Interval of time (in seconds), after which the bot deletes it's message (and command message) which is expected to be viewed instantly. Note: Set to -1 to never automatically delete messages
- **IS_TEAM_DRIVE**: (Optional field) Set to `True` if GDRIVE_FOLDER_ID is from a Team Drive else False or Leave it empty.
- **USE_SERVICE_ACCOUNTS**: (Optional field) (Leave empty if unsure) Whether to use service accounts or not. For this to work see "Using service accounts" section below.
- **INDEX_URL**: (Optional field) Refer to https://github.com/maple3142/GDIndex/ The URL should not have any trailing '/'
- **API_KEY**: This is to authenticate to your telegram account for downloading Telegram files. You can get this from https://my.telegram.org DO NOT put this in quotes.
- **API_HASH**: This is to authenticate to your telegram account for downloading Telegram files. You can get this from https://my.telegram.org
- **MEGA_KEY**: Mega.nz api key to mirror mega.nz links. Get it from [Mega SDK Page](https://mega.nz/sdk)
- **MEGA_USERNAME**: Your mega email id (You can leave it empty, it will start megasdkrest server in anonymous mode)
- **MEGA_PASSWORD**: Your password for your mega.nz account. (**NOTE**: You must deactivate 2FA to use the account with the bot otherwise bot will not be able to sign in)
- **STOP_DUPLICATE_MIRROR**: (Optional field) (Leave empty if unsure) if this field is set to `True` , bot will check file in drive, if it is present in drive, downloading will ne stopped. (Note - File will be checked using filename, not using filehash, so this feature is not perfect yet)
- **ENABLE_FILESIZE_LIMIT**: Set it to `True` if you want to use `MAX_TORRENT_SIZE`.
- **MAX_TORRENT_SIZE**: To limit the torrent mirror size, Fill The amount you want to limit, examples: if you fill `15` it will limit `15gb`.
- **BLOCK_MEGA_FOLDER**: (Optional field) If you want to remove mega.nz folder support, set it to `True`.
- **BLOCK_MEGA_LINKS**: (Optional field) If you want to remove mega.nz mirror support (bcoz it's too much buggy and unstable), set it to `True`.
- **UPTOBOX_TOKEN**: Uptobox token to mirror uptobox links. Get it from [Uptobox Premium Account](https://uptobox.com/my_account).
- **SHORTENER_API**: Fill your shortener api key if you are using shortener.
- **SHORTENER**: (Optional field) if you want to use shortener in Gdrive and index link, fill shotener url here. Examples:
```
exe.io
gplinks.in
shrinkme.io
urlshortx.com
shortzon.com
```

**Note**: Above are the supported url shorteners. Except these only some url shorteners are supported. If you want to use any other url shortener then first ask me that shortener is supported or not.

</details>

## Getting Google OAuth API credential file

- Visit the [Google Cloud Console](https://console.developers.google.com/apis/credentials)
- Go to the OAuth Consent tab, fill it, and save.
- Go to the Credentials tab and click Create Credentials -> OAuth Client ID
- Choose Desktop and Create.
- Use the download button to download your credentials.
- Move that file to the root of mirrorbot, and rename it to credentials.json
- Visit [Google API page](https://console.developers.google.com/apis/library)
- Search for Drive and enable it if it is disabled
- Finally, run the script to generate **token.pickle** file for Google Drive:
```
pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib
python3 generate_drive_token.py
```

## Deploying

- Start docker daemon (skip if already running):
```
sudo dockerd
```
- Build Docker image:
```
sudo docker build . -t mirrorbot
```
- Run the image:
```
sudo docker run mirrorbot
```

## Deploying on Heroku

Fork this repo then upload **token.pickle** to your forks

**NOTE**: If you didn't upload **token.pickle**, uploading will not work.
<p><a href="https://heroku.com/deploy"> <img src="https://img.shields.io/badge/Deploy%20To%20Heroku-blueviolet?style=for-the-badge&logo=heroku" width="200""/></a></p>

## Deploying on Heroku using heroku-cli
<details>
    <summary><b>Click here for more details</b></summary>

- Install [Heroku cli](https://devcenter.heroku.com/articles/heroku-cli)
- Login into your heroku account with command:
```
heroku login
```
- Create a new heroku app:
```
heroku create appname
```
- Select This App in your Heroku-cli: 
```
heroku git:remote -a appname
```
- Change Dyno Stack to a Docker Container:
```
heroku stack:set container -a appname
```
- Add Private Credentials and Config Stuff:
```
git add -f credentials.json token.pickle config.env heroku.yml
```
- Commit new changes:
```
git commit -m "Added Creds."
```
- Push Code to Heroku:
```
git push heroku master --force
```
- Restart Worker by these commands,You can Do it manually too in heroku.
- For Turning off the Bot:
```
heroku ps:scale worker=0 -a appname
```
- For Turning on the Bot:
```
heroku ps:scale worker=1 -a appname		
```

</details>

## Bot commands to be set in [@BotFather](https://t.me/BotFather)

```
start - Start the bot
mirror - Start Mirroring
tarmirror - Upload tar (zipped) file
unzipmirror - Extract files
clone - copy file/folder to drive
watch - mirror YT-DL support link
tarwatch - mirror youtube playlist link as tar
authorize - Authorize a group chat or a specific user
unauthorize - Unauthorize a group chat or a specific user
cancel - Cancel a task
cancelall - Cancel all tasks
del - Delete file from Drive
list - [query] searches files in G-Drive
status - Get Mirror Status message
stats - Bot Usage Stats
ping - Ping the bot
restart - Restart the bot
help - Get Detailed Help
speedtest - Check Speed of the host
log - Bot Log [owner only]
repo - Get the bot repo
config - Edit 'config.env' file
```

## Using service accounts for uploading to avoid user rate limit
For Service Account to work, you must set **USE_SERVICE_ACCOUNTS="True"** in config file or environment variables
Many thanks to [AutoRClone](https://github.com/xyou365/AutoRclone) for the scripts
**NOTE**: Using service accounts is only recommended while uploading to a team drive.

## Generate service accounts. [What is service account](https://cloud.google.com/iam/docs/service-accounts)

Let us create only the service accounts that we need. 
**Warning**: abuse of this feature is not the aim of this project and we do **NOT** recommend that you make a lot of projects, just one project and 100 sa allow you plenty of use, its also possible that over abuse might get your projects banned by google. 

```
Note: 1 service account can copy around 750gb a day, 1 project can make 100 service accounts so that's 75tb a day, for most users this should easily suffice. 
```

`python3 gen_sa_accounts.py --quick-setup 1 --new-only`

A folder named accounts will be created which will contain keys for the service accounts

**NOTE**: If you have created SAs in past from this script, you can also just re download the keys by running:
```
python3 gen_sa_accounts.py --download-keys project_id
```

## Add all the service accounts to the Team Drive
- Run:
```
python3 add_to_team_drive.py -d SharedTeamDriveSrcID
```

## Youtube-dl authentication using .netrc file
For using your premium accounts in youtube-dl, edit the [.netrc](https://github.com/breakdowns/slam-mirrorbot/blob/master/.netrc) file according to following format:
```
machine host login username password my_youtube_password
```
where host is the name of extractor (eg. youtube, twitch). Multiple accounts of different hosts can be added each separated by a new line

# Support Group
<p><a href="https://t.me/PriiiyoMirror"> <img src="https://img.shields.io/badge/Priiiyo%20Mirror%20-black?style=for-the-badge&logo=telegram" width="230""/></a></p>

# Credits

Thanks to:
- [out386](https://github.com/out386) heavily inspired from telegram bot which is written in JS
- [Izzy12](https://github.com/lzzy12/) for original repo
- [magneto261290](https://github.com/magneto261290/) for some features
- [ksssomesh12](https://github.com/ksssomesh12/tg-mirror-bot) for some special features
- [SVR666](https://github.com/SVR666/) for some features & fixes
- [Breakdowns](https://github.com/breakdowns/slam-mirrorbot) for some cool modules

and many more people who aren't mentioned here, but may be found in [Contributors](https://github.com/priiiyo/priiiyo-mirror-bot/graphs/contributors).
