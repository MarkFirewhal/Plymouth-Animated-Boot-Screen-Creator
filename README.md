# Animated Boot Screen Shell Script

This is a template built off of Eonix's animated cat plymouth theme. I modified some of jcklpe's scripts and added a new Plymouth theme for my laptop.  

![Bootscreen](https://i.imgur.com/0ReRjOf.gif)


## Instructions 

1. Install Plymouth themes:

`sudo apt install plymouth-themes`

2. Go to the plymouth themes folder

`
cd /usr/share/plymouth/themes
`

3. Create new theme folder

`
sudo mkdir ./{{ThemeNameHere}}
`

4. Move into your new theme folder.

`
cd {{ThemeNameHere}}
`

3. Clone this repo contents into the folder.

`
sudo git clone https://github.com/MarkFirewhal/Plymouth-Animated-Boot-Screen-Creator.git .
`
The dot at the end will clone the contents of the template folder into your new theme folder. 

### Installation
1. Install the theme by creating a symbolic linking from your new theme to the default.python file, be sure to replace the template variables used:

```
 sudo update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth /usr/share/plymouth/themes/{{ThemeName}}/{{ThemeName}}.plymouth 100
```
In my case it looks like this:
```
sudo update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth /usr/share/plymouth/themes/laptop/laptop.plymouth 100
```

2. Select the default theme.
`sudo update-alternatives --config default.plymouth`
If nothing appears, you're done. 

But if you see a list of themes, select the number for your theme. 
Be sure to adjust the priority in the command above (100) to something larger than the other themes so auto can take over. Or not, whatever. 

2.5 Double check that theme install.
If you need to search the list of themes, just to be sure, before you commit... use:
`
plymouth-set-default-theme -l
`
To set the actual theme and automagically re-create the initramfs:
`
plymouth-set-default-theme -R laptop
`
Edit the defaults file to point to your new theme, incase the above didnt work:
`
sudo nano /usr/share/plymouth/plymouthd.defaults
`

3. Update the initramfs image.

`
sudo update-initramfs -u
`

4. Modify Grub.

`
sudo nano /etc/default/grub
`
Find the line: "GRUB_CMDLINE_LINUX_DEFAULT" = add the following: `quiet splash break=init`
NEXT,
Find the line: "GRUB_GFXMODE" = change it to the following: `1024x768` (or whatever your native screen resolution is)

`
sudo update-grub2
`

(NOTE): if you're having trouble with grub at all, install the GUI: 
`
sudo apt install grub-customizer
`


5. Now reboot.

If you want to install this on < Ubuntu 16.04, change the path from /usr/share/plymouth to /lib/plymouth/ . You need to do this on the laptop.plymouth file also.

Also other possible perks:

I found some scripts that are supposed to improve the transitions etc between Plymouth and the other parts of the start up process. I don't know if they worked for me or not. I had trouble getting the plymouth-gdm thing working as it seems that it's primarily something to do with pre-gdm3 versions and I'm on Ubuntu 18.04

Anyway:

- Edit the file /etc/initramfs-tools/conf.d/splash and add this line:

`
FRAMEBUFFER=y
`

- To enable smooth transition you have to disable your display manager unit, e.g.

`
systemctl disable gdm.service
`

- Enable the respective DM-plymouth Unit (GDM, LXDM, SLiM units provided), e.g.

`
systemctl enable gdm-plymouth.service
`



### Customization

1. Put mp4 into the "input" folder. 
2. Run the mp4-to-png.sh file for the filename you are using. 
3. Transfer the image sequence to the root of the project folder.
4. Open up animated-boot.script and change the lines:
```
for (i = 0; i < {{NUMBER OF IMAGES IN SEQUENCE}}; i++)
  flyingman_image[i] = Image("progress-" + i + ".png");
flyingman_sprite = Sprite();
```
and 
```
flyingman_sprite.SetImage(flyingman_image[Math.Int(progress / 3) % {{NUMBER OF IMAGES}}]);
```
To have the correct number of images in the sequence. 

5. Resave the animated-boot.script file. 
6. Open up laptop.plymouth and enter in your name, descriptions, and new script paths etc and save as {{ThemeNameHere}}.plymouth



### Additional Notes
Plymouth can be opened in Linux MX with:
`
sudo mx-boot-options
`


### Credits

Mp4 recorded with: https://github.com/NickeManarin/ScreenToGif

Original repo by Eionix: https://github.com/krishnan793/PlymouthTheme-Cat/commits?author=krishnan793

[![Video](https://i.imgur.com/0ReRjOf.gif)](https://www.youtube.com/watch?v=c6f478VBhtE)

[Video] https://www.youtube.com/watch?v=c6f478VBhtE

[Blog] http://eionix.blogspot.in/2016/10/plymouth-theme-for-ubuntu.html

And if you want to check out m jcklpe's stuff, visit https://www.jackalope.tech/blog
