# i3lock-multimonitor

This script can be used as an i3 window manager lock script on mutli-monitor
setups. It is able to sense the screen resolutions and resizes a given image
appropriately for the screens. Next, it applies a blur and dim effect, after
which it will render a rectangular image on each screen for the OSD to be
projected on.

The idea for this project was shamelessly copied from [guimeira](https://github.com/guimeira)'s [i3lock-fancy-multimonitor](https://github.com/guimeira/i3lock-fancy-multimonitor).

It uses [ImageMagick](http://www.imagemagick.org/) to resize the [background image](./img/background.png). You can replace this image to change background.

By using information from [xrandr](http://www.x.org/wiki/Projects/XRandR/) and
basic math, this script supports multiple monitor setups, displaying the
background image on all screens.

It caches the generated image for different screen sizes and xrandr output. So even though first `lock` command will take a second to finish, subsequent `lock` will be lighting fast. This delay may be circumvented by pre-caching images.

## Installation

Make sure you have all the dependencies (example on Arch Linux):

	$ sudo pacman -S imagemagick i3lock

Copy the `lock` script along with the images to some place on your system (e.g.: the i3 folder) and give it execution permission:

```
git clone https://github.com/shikherverma/i3lock-multimonitor.git
cp -r i3lock-multimonitor ~/.i3mm
chmod +x ~/.i3mm/i3lock-multimonitor/lock
```

Create a key binding on your i3 config file (in this example I'm using $mod+p):

```
echo "bindsym \$mod+p exec /home/$USER/.i3/i3lock-multimonitor/lock" >> ~/.i3/config
```

Now reload the i3 config file. By default, the key binding is `$mod+Shift+c`.

## Usage

The script can be invoked in multiple ways.

### Cache mode

This will update the image cache but not lock the screen. It may take a few
seconds to complete.

	$ lock -c /path/to/image.jpg

### Lock mode

This will lock the screen. If a cached image already exists, the screen will be
locked right away, otherwise an image will be generated and cached.

	$ lock -l /path/to/image.jpg

It is always preferable to have the images pre-cached to make the locking as
quick and seamless as possible. Also remember that different images are cached
for single and multimode setups.
