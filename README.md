# My KDE Theme

Heavily inspired by PearOS, MacOS & CutefishOS

---

## Sections

- [MetaData](#metadata)
- [Screenshots](#screenshots)
- [Steps](#steps)

---

## Metadata

```
UBUNTU_VERSION_ID="24.04"
KDE_VERSION="6.3"
```

---

## Screenshots

soon...

--

## Steps

### 1. Change The Fonts (and Emojis)

- You can download the fonts you like, I like Apple fonts tbh (sorry open-source community, I have failed you), I downloaded these fonts
    - [SF Pro](https://devimages-cdn.apple.com/design/resources/download/SF-Pro.dmg) (sans-serif)
    - [SF Pro Arabic](https://devimages-cdn.apple.com/design/resources/download/SF-Arabic.dmg) (sans-serif, for Arabic text support)
    - [New York](https://devimages-cdn.apple.com/design/resources/download/NY.dmg) (serif)
    - [Monaspice](https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Monaspace.zip) (monospace)
    - +3 emoji fonts (to have the freedom of switching later)
        - Apple Color Emoji (from [this Telegram channel](https://t.me/EmojiReplacer))
        - Facebook MFFM Emoji (from [this Telegram channel](https://t.me/mffmemojis))
        - WhatsApp MFFM Emoji (from [this Telegram channel](https://t.me/mffmemojis))
- To download other Apple fonts have a look [here](https://developer.apple.com/fonts/)
- Extract `.dmg` Apple fonts by opening the terminal on the fonts directory (usually `~/Downloads/`) and writing these commands (Note that this is for Apple .dmg files ONLY, if you downloaded a ttf/otf font you can skip this step)

```bash
# extract .dmg file
7z x <DMG_FILE>.dmg

# navigate to the newly extracted directory
cd <FONT_DIR>/

# extract the .pkg file
7z x <PKG_FILE>.pkg

# extract the newly created `Payload~` file
7z x Payload~

# navigate to the newly created directory
cd Library/

# You will find your fonts right away
```

- Save the font family names in a place (we will need them later), you can run this command to a `.ttf` or `.otf` file

```bash
# Example
fc-scan NewYork.ttf

# output
# family: "New York"(s) # THIS IS WHAT WE WANT => "New York"
# familylang: "en"(s)
# ...
```

- move your desired font files (ttf/otf files) to install them

```bash
# DO ((((ONLY ONE)))) OF THESE 2 COMMANDS
# Personal Recommendation, if you are a beginner, do command 1

# COMMAND 1: Install fonts for this user only
mkdir -p ~/.local/share/fonts/
mv <FONTFILE>.ttf ~/.local/share/fonts/ # otf fonts are acceptable also

# COMMAND 2: Install fonts systemwide
sudo mv <FONTFILE>.ttf /usr/share/fonts/ # otf fonts are acceptable also
```

- Remember to do this for all the fonts you want to apply / be able to use ✅
- Emoji fonts are no different than any other font, they come in ttf/otf as well
- Apply the fonts by going to `System Settings > Apperance & Style > Text & Fonts` and change the font to the desired font (usually a sans-serif font), this font will be the dominant font in most of the text (don't worry about the Arabic, monospace and Emoji fonts, theya're in the next steps)
- Build the fonts cache by writing `fc-cache -f -v` in the terminal
- To apply changes you do ONE of the following
    - Open `KRunner` (alt + F2) and typing `plasmashell --replace`
    - Reboot the device
- Create a new font configurations directory

```bash
mkdir -p ~/.config/fontconfig/conf.d/ # current user

# For system wide, no need because the directory is already there
```

- Change the default monospace emoji **(if not done from Graphical UI)** by creating a new configuration file

```bash
# go to the newly created directory (ONLY ONE COMMAND OF THE 2)

# COMMAND 1
cd ~/.config/fontconfig/conf.d/ # current user

# COMMAND 2
cd /usr/share/fontconfig/conf.avail/ # system wide

# create a new file
nano 70-default-monospace-font.conf # you can rename it with whatever, but it must start with 7 or 6 [not sure] and must end with .conf
```

- paste this content in the `.conf` file

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <alias>
        <family>monospace</family>
        <prefer>
            <!-- Put font family name you get from `fc-scan` command -->
            <family>[FONT_NAME_HERE]</family>
        </prefer>
   </alias>
</fontconfig>
```

- Create another `.conf` file to change default serif font if you downloaded a serif font

```bash
# same way as before (go to conf.d/ or conf.avail/ directory)

nano 70-default-serif-font.conf
```

- paste this content

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <alias>
        <family>serif</family>
        <prefer>
            <!-- Font name you got from `fc-scan` command -->
            <family>[FONT_NAME]</family>
        </prefer>
   </alias>
</fontconfig>
```

- Create additional configuration file for default Arabic (or any specific locale) font

```bash
nano 70-default-arabic-font.conf
```

- paste this content

```xml
<?xml version='1.0'?>
<!DOCTYPE fontconfig SYSTEM 'fonts.dtd'>
<fontconfig>
    <match>
        <test compare="contains" name="lang">
            <!-- For arabic => ar, for Japanese => jp, etc... -->
            <string>[LOCALE_ISO2_CODE]</string>
        </test>
        <edit mode="prepend" name="family">
            <string>[FONT_FAMILY_NAME]</string>
        </edit>
    </match>
</fontconfig>
```

- One last configuration file, default emojis

```bash
nano 70-default-emoji-font.conf
```

- Paste this content

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">

<fontconfig>
    <match>
        <test name="family">
        <string>sans-serif</string></test>
        <edit name="family" mode="prepend" binding="strong">
            <string>[EMOJI_FONT_NAME]</string>
        </edit>
    </match>
    <match>
        <test name="family">
            <string>serif</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
            <string>[EMOJI_FONT_NAME]</string>
        </edit>
    </match>
    <match>
        <test name="family">
            <string>monospace</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
            <string>[EMOJI_FONT_NAME]</string>
        </edit>
    </match>
    <match>
        <test name="family">
            <string>emoji</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
            <string>[EMOJI_FONT_NAME]</string>
        </edit>
    </match>
</fontconfig>
```

- Rebuild the cache again in the terminal

```bash
fc-cache -f -v
```

- Restart the shell in KRunner (or Reboot the device)

```bash
# in KRunner NOT the terminal
plasmashell --replace
```

// later

https://github.com/Pear-Project/pearOS-Default-Aurorae

cp -r ./* ~/.local/share/aurorae/themes/

https://github.com/Pear-Project/pearOS-Default-Icons


For KVantum

https://github.com/tsujan/Kvantum/blob/master/Kvantum/doc/Theme-Config

Lines from 0 to 20

